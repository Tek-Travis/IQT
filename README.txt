Internet Quality Test
Internet Quality Test is a browser-based connection stability monitor designed to give users a clear, live picture of how responsive and reliable their internet connection feels during normal web use. It runs entirely in the browser as a single HTML file and is intended to work on GitHub Pages or any other static web host. There is no backend server, no user account system, no database, no analytics platform, no API key, and no software to install. A visitor opens the page, presses Start 5-Min Test, and the browser performs a controlled five-minute series of lightweight HTTPS checks.

Ownership and Use Notice
Internet Quality Test is an original application concept, design, and custom implementation created by Travis John Richard.

Copyright © 2026 Travis John Richard. All rights reserved.

This project, including but not limited to its source code, HTML, CSS, JavaScript, algorithms, measurement workflow, three-gearbox processing model, 0–100 scoring system, gauge design, quality zones, visual layout, branding, stability-strip behavior, interaction design, written documentation, and all related original materials, is proprietary to Travis John Richard.

No part of this application may be copied, reproduced, duplicated, distributed, republished, adapted, modified, reverse-engineered, incorporated into another project, displayed publicly, sold, licensed, transmitted, or reused in any form without the prior written permission of Travis John Richard.

Any use of this application or its materials—including commercial use, personal reuse, educational redistribution, template reuse, code reuse, derivative works, screenshots used for product replication, or adaptation of its original design and functionality—is strictly forbidden unless explicit written permission has been granted in advance by the owner.

Permission requests must be submitted in writing to:

Travis John Richard
optimaloptimusprimus@hotmail.com

Unauthorized reproduction, redistribution, or reuse is prohibited.

Purpose
The application is not a traditional download-speed or upload-speed test. It does not attempt to measure the maximum megabits per second a connection can transfer, and it does not claim to provide ICMP ping, IP packet-loss, router diagnostics, Wi-Fi signal analysis, or a complete diagnosis of an ISP or network path.

Its purpose is more specific: it measures whether a web browser can repeatedly complete lightweight HTTPS requests in a timely, stable, and reliable manner. This makes the application useful for identifying general browsing responsiveness, intermittent lag, inconsistent web performance, slow responses, browser-visible interruptions, and recurring request failures.

Internet Quality Test is intended to show how a connection behaves across a meaningful short session rather than relying on a single isolated measurement.

Five-minute test
The test runs for five minutes. During that session, the application schedules one browser HTTPS probe approximately every five seconds.

text
5 minutes
× 60 seconds per minute
÷ 5 seconds per scheduled probe
= up to 60 scheduled probe slots
Each finalized slot becomes one position in the stability strip at the bottom of the screen. A completed session therefore gives the user a timeline of the connection’s behavior over the complete five-minute test.

The test starts only after the visitor presses the Start 5-Min Test button. It can be stopped manually at any time, and it pauses when the browser tab becomes hidden. A hidden-tab pause does not count as a connection failure or Probe Loss event.

What the app measures
The application measures browser-visible HTTPS responsiveness and stability. It displays three direct measurements:

Ping is the most recent successful HTTPS request duration.

Latency is the median timing from up to six recent successful comparable probes from the same source.

Jitter is the average amount of timing change between recent successful comparable requests.

It also calculates internal quality signals:

Probe reliability measures whether scheduled browser probes are returning successfully.

Probe Loss identifies scheduled probes that the browser could not complete after retry and failover handling.

Spike behavior identifies slow and severe completed request events.

Recovery behavior evaluates whether normal responses return after an instability event.

The monitor does not read, show, save, upload, or analyze the body content returned by tested websites. It only records whether the browser completed the request and, if so, how long that attempt took.

What the app does not measure
The monitor deliberately avoids claims it cannot verify. It does not measure:

Download bandwidth.

Upload bandwidth.

ICMP ping.

IP-layer packet loss.

Router performance.

Wi-Fi signal strength.

Exact physical network-path conditions.

Complete ISP, device, or destination-server diagnosis.

A red event in the stability strip means Probe Lost, not confirmed packet loss. Probe Loss means the browser did not complete the configured HTTPS request after the allowed retry and source-failover process.

A browser request can fail because of local connection trouble, an endpoint problem, a captive portal, DNS trouble, browser policy, a content filter, network restrictions, device issues, or other factors. The application therefore uses accurate wording and does not claim that a red slot proves IP packet loss.

HTTPS probing process
The application sends lightweight browser HTTPS requests to public connectivity-style endpoints. It can use sources associated with Google, Mozilla, and Microsoft.

The normal measurement process is:

The application begins with one active HTTPS source.

It sends one lightweight browser request.

If the request returns, the result is classified by response duration.

If the request fails, the application waits briefly and retries the same source once.

If the retry also fails, that source enters a temporary cooldown period.

The application attempts one alternate available source.

The complete retry/failover sequence becomes one finalized scheduled slot.

The application does not run normal requests in parallel. Each scheduled five-second interval results in only one stability-strip record, preventing retries from artificially creating extra test results.

Requests use cache-busting values and a no-store cache policy to reduce the chance that the browser reuses an old cached response. They also use omitted credentials, meaning the test does not intentionally include cookies, login sessions, account information, or other browsing credentials. Each individual request attempt has a four-second timeout.

Three-gearbox system
Internet Quality Test processes probe data through three stages, called gearboxes. This model prevents a single raw response from making the quality score or dial behave unpredictably.

Gearbox 1: Probe Conditioner
The first gearbox turns each scheduled interval into one clean, finalized probe record. Every record includes the slot number, source used, request timing when successful, final result classification, retry information, and failover context.

A returned request is classified as normal, slow, or severe. A request that does not complete after its allowed retry and fallback process is classified as lost.

Normal requests return in 200 milliseconds or less. Slow requests return above 200 milliseconds but below 750 milliseconds. Severe requests return at 750 milliseconds or more. Lost requests do not complete as browser HTTPS probes.

This gearbox does not average away or conceal failed events. It records what the browser experienced during the session.

Gearbox 2: Metric Stabilizer
The second gearbox turns the raw probe history into stable measurements.

Ping is the latest successful request time. Latency is calculated from the median of up to six recent successful probes from the same source. Using the median reduces the chance that one unusually slow but completed request will distort the estimate of typical current latency.

Jitter is calculated from recent timing differences between comparable successful requests. Low jitter means timing remains steady. Higher jitter means request timing is jumping around, which can make browsing, calls, gaming, and other interactive use feel unstable even when average latency appears acceptable.

The second gearbox also reviews the latest twelve finalized slots, representing about one minute of recent history. This one-minute window evaluates Probe Loss, reliability, slow responses, severe lag spikes, and recovery after disruptions.

Gearbox 3: Quality Score Engine
The third gearbox converts stable measurements into the 0–100 Quality Score.

text
Reliability and Probe Loss: 30%
Latency:                    25%
Jitter:                     20%
Slow and severe spikes:     20%
Recovery behavior:           5%
The score system is designed to balance responsiveness with stability. A connection should not receive an Excellent score merely because one request was fast if other recent requests are failing, fluctuating, or experiencing severe lag spikes.

The app also applies protective score caps. These prevent a good average from hiding a current reliability problem. For example, one lost probe in the latest twelve slots limits the maximum score to 89. Two lost probes limit it to 74. Two consecutive lost probes limit it to 54. Confirmed failures across two sources limit the score to 39. Three consecutive lost probes limit it to 29. If all configured sources are unavailable, the score cannot exceed 15.

Quality gauge
The main display is a true upper semicircle gauge from 0 to 100.

text
0   = needle points left
50  = needle points upward
100 = needle points right
The 0 and 100 values are aligned at the same horizontal level. The gauge does not use a full circular 360-degree range.

The score ranges are:

Critical: 0–29

Poor: 30–54

Fair: 55–74

Good: 75–89

Excellent: 90–100

These quality words are part of the gauge itself. The needle points into the active colored segment, the matching quality label brightens, and the center of the gauge repeats both the exact numeric score and its quality level.

For example, a score of 88 points into the Good range. The Good arc segment and Good label illuminate, while the center of the gauge displays 88 and GOOD.

Smooth score movement
The score calculation and the visible dial movement are deliberately separate.

After each finalized probe, the quality engine calculates a new target score. A dedicated browser animation loop then moves the visible needle and score number smoothly toward that target. This avoids jerky movement caused by restarting a visual transition every time a probe completes.

Small changes under one point are ignored visually so the gauge does not twitch over harmless variation. Score decreases are shown more quickly than score recoveries. Serious confirmed trouble causes a faster movement toward the protected lower score, while still easing into position instead of snapping harshly.

Visitors with reduced-motion preferences enabled receive direct, non-animated display updates.

Stability strip and explanations
The bottom stability strip contains sixty slots. Each slot represents one scheduled five-second probe interval.

Green means the request completed normally. Yellow means it completed but was slow. Orange means it completed with a severe lag spike. Red means Probe Lost. Icy blue means a probe is currently active. Muted blue-gray means that future test interval has not happened yet.

Each completed slot can be hovered with a mouse, focused with the keyboard, or tapped on a touch device. The application gives a plain-language explanation beneath the strip.

Examples include:

text
Slot 14 · Healthy · 46 ms HTTPS response.

Slot 29 · Slow · 318 ms response.
Higher than the normal-response threshold.

Slot 34 · Severe · 842 ms response.
This counts as a major lag spike.

Slot 41 · Probe Lost.
The browser did not complete the HTTPS check after retry/failover.
This is not confirmed IP packet loss.