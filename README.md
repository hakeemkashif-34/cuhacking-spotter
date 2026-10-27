# spotr

**Best Marketing Hack at cuHacking 7**, Carleton University's official hackathon (July 2026).

spotr is an AI spotter for the gym floor. A camera watches the bar, and when a lift starts to fail, spotr warns gym staff before anyone across the room would notice.

We built it as a team of 4 in 2 days.

## The Problem

A lot of weight-training injuries happen when a rep fails and nobody is close enough to help. Gym staff can't watch every rack at once.

spotr watches the racks for them, and speaks up when a lift goes wrong.

## How It Works

1. **Camera:** a Raspberry Pi camera tracks a weight plate's position through each lift.
2. **Bar speed:** the plate's movement is turned into the bar's relative velocity.
3. **Detection:** rolling z-scores against the lifter's own median bar speed flag any rep that slows down sharply. So spotr compares you to your own normal, not to a fixed number.
4. **Severity report:** each flagged rep goes to the Gemini API, which writes a graded severity report in real time instead of a simple pass or fail.
5. **Spoken alert:** the ElevenLabs API reads that report out loud for gym staff.
6. **Staff dashboard:** staff see every rack live, and acknowledge alerts as they come in.

The Pi runs **QNX**, a real-time operating system, to keep data capture steady under continuous load. That mattered more than we expected once we started stress testing right before submission.

## What's in This Repo

This repo holds the two front-end pages we built for the demo:

1. `spotr-demo.html`: the **Live Monitor**, for a single rack
2. `spotr-dashboard.html`: the **Fleet Dashboard**, for staff watching every rack

Both are plain HTML, CSS, and JavaScript, with no frameworks or libraries. Both run on simulated data, with controls so judges could trigger each scenario live. The camera and backend code isn't in this repo.

### Live Monitor

1. **Four states:** Idle, Active Lift, Warning, and Alarm.
2. **Auto-escalation:** a stalled lift moves to Warning, and if it isn't resolved within 6 seconds, it escalates to Alarm on its own.
3. **Colour by state:** the whole screen shifts between green, blue, amber, and red through CSS custom properties.
4. **Animated camera view:** an SVG lifter squats with a barbell. A pose model works out every joint from the squat depth, and each rep follows a smooth cosine curve.
5. **Live stats:** rep count, bar speed in m/s, bar height in metres, and a lift timer that turns into a countdown during Warning.
6. **Rack LED:** a glowing light that matches the current state, standing in for a real light on the rack.

### Fleet Dashboard

1. **Lifter check-in:** lifters type their name or pick a recent one, then their session starts on Bench Press 1.
2. **Locations tab:** a live tile for each of 3 bench press racks, showing reps and lift time, or how long a rack has been in an emergency.
3. **Staff tab:** one lane per rack, an Acknowledge button, and search by lifter name.
4. **Two alert levels:**\
    a) A stall raises a Level 1 emergency, with a pop-up that closes after 5 seconds.\
    b) After 6 seconds without help, it becomes Level 2, with a pop-up that stays until staff respond.\
    c) Acknowledging an alert stops the escalation.
5. **Incident log:** a timestamped record of every stall, escalation, acknowledgement, and reset.
6. **Rack ordering:** racks with an emergency move to the top.
7. **Reports tab:** each lifter's last session, today's reps and clean reps, average speed, and personal bests.
8. **Charts:** sets per week and % clean reps, drawn by hand in SVG with hover details.

## Running It

1. Download or clone this repo.
2. Open `spotr-demo.html` or `spotr-dashboard.html` in any browser. There's nothing to install or build.
3. Use the on-screen buttons, or press keys 1 to 4:\
    a) **1:** Start Lift\
    b) **2:** Trigger Stall\
    c) **3:** Resolve\
    d) **4:** Reset

## Tech

1. **Hardware:** Raspberry Pi, Pi camera, QNX
2. **Detection:** rolling z-scores with a median baseline
3. **AI:** Gemini API, ElevenLabs API
4. **Front end:** HTML, CSS, JavaScript, SVG

## Team

1. [Hakeem Kashif](https://www.linkedin.com/in/hakeem-kashif/)
2. [Vish Rajesh](https://www.linkedin.com/in/vish-rajesh-99b1272a7/)
3. [Arham Siddiqi](https://www.linkedin.com/in/arhamsiddiqi/)
4. [Kaden Pang](https://www.linkedin.com/in/kaden-pang/)
