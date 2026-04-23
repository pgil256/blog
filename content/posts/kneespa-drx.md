---
title: "My E-Stop Didn't Actually Stop"
date: 2026-04-23T10:00:00-05:00
tldr: "Shipped a medical device for nine months with zero Python tests. Finally sat down to write pytest, and it immediately found that my emergency stop didn't stop, my serial layer was eating its own ACKs, and the 0-degree calibration mark was set to 0 instead of 1390. Write the tests. Write them earlier than I did."
description: "What writing a test suite nine months late taught me about the Kneespa DRx."
tags: [testing, embedded, kneespa]
---

The Kneespa DRx is a knee decompression machine. A Pi drives a PyQt5 touchscreen app, the app drives an Arduino over serial at `/dev/serial0`, and the Arduino drives three actuators — axial, horizontal, and lateral — pressing against somebody's knee. There is a pressure cap hard-coded at 80 lbs and a physical e-stop button and I thought about both of them constantly.

I did not think about tests.

From the first commit in May 2025 to about February 2026, the Python side of this thing had zero unit tests. Zero. I ran it on the hardware, I trusted the hardware to yell at me when I was wrong, and I shipped features. Protocol 4 landed in July with 30-second oscillating cycles. I wrote a little vanilla-JS demo of the knee for the portfolio. I cleaned `node_modules/` out of the repo (don't ask). Eight months of feature work on a medical device with no unit tests.

Then I sat down and wrote them. Three commits over two weeks: a design doc, an 18-task TDD plan, then the suite itself — pytest, pytest-qt, a pty-backed `FakeArduino`, and PlatformIO mocks for the firmware side. 105 tests passing.

And *the second* those tests could run, they lit the codebase up like a Christmas tree.

## The three things I found

**1. The E-Stop didn't actually stop.**

`run_pressure_sequence` and `set_to_pressure` in `main/helpers/protocols.py` both have wait loops — "sleep 50ms, check pressure, sleep 50ms, check pressure, repeat until target." Those loops were never checking `self.is_running`. So if you hit e-stop mid-ramp, the protocol thread would keep pressurizing until the loop naturally expired on its own schedule. The UI said STOPPED. The motor did not.

Fix: one line at the top of every loop.

```python
while self.current_pressure < target:
    if not self.is_running:
        return
    # ...
```

Three characters of condition per loop, and the thing that was supposed to save a knee had not been saving a knee. I have written more humbling code than that. But not much more.

**2. My serial layer was eating its own ACKs.**

Every call to `send()` in `main/helpers/arduino.py` started with `self.serial_com.reset_input_buffer()`. My thinking: "clear any stale bytes before I write the next command." My actual behavior: "dump whatever the Arduino just sent me before I ever read it." The reader thread was racing the flush and on any slow frame it lost. Status updates disappeared. `"OK"` acknowledgments disappeared. The keepalive `T` command was timing out because the keepalive's *own reply* was getting flushed by the next outgoing command.

I ripped the flush out. Ninety percent of the "it just feels laggy" complaints I had been making to myself for a year turned out to be that one line.

While I was in there, I added a `threading.Event` called `_reader_ready` so `connect_to_arduino()` actually waits for the reader thread to be listening before it calls `verify_connection()`. That killed a startup race that had been hiding inside a 10-second timeout. And I wrapped `current_pressure` and `current_pos_c` in property getters/setters behind a `_state_lock` because the reader thread and the protocol thread were racing on shared ints. I had not seen the race. The tests saw the race.

**3. CMarks 0-degree was literally 0.**

`main/config/kneespa.cfg` is an INI file with calibration marks for the lateral actuator. 0 degrees should correspond to encoder position ~1390. It said:

```ini
[CMarks]
0.0 = 0
```

The reset routine homes the lateral actuator to `CMarks[0.0]`. Which was 0. Which is off the bottom end of the lateral range — valid range is 500 to 2400. So every time the device reset, the lateral actuator drove past its mechanical limit until something gave. I had been hearing it clunk for months and telling myself "it's just the home routine."

The fix is one line in a config file and it is the most embarrassing patch I have ever written:

```diff
-0.0 = 0
+0.0 = 1390
```

## What I actually learned

The tests didn't *create* these bugs. The tests revealed bugs I had been writing around for nine months. The serial flush had been eating ACKs since day one. The e-stop had never worked. The CMarks value was wrong from the very first checkin. I had just been compensating — wrapping longer timeouts around the flush problem, telling myself the reset clunk was normal, assuming the e-stop worked because the UI said so.

A few things I'm internalizing:

- **"Works on the hardware" is not the same as "works."** The hardware does not tell you when your software is compensating for itself.
- **Write the harness before you write the fourth protocol.** `FakeArduino` with a pty took me an afternoon. Nine months of tests would have been an afternoon of nine months ago.
- **Every wait loop on a machine that touches a person needs an `is_running` check.** Non-negotiable from here forward.
- **Configuration is code.** A calibration file I never tested is code I never tested.

Next on the list is an adaptive pressure ramp that responds to patient feedback. I am writing the tests first. For the love of God, write the tests first.
