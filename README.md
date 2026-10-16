# WALL-E and EVE's Signal

A browser-based, seeded search simulation. WALL-E receives a starting signal, explores a partially observed grid, finds clues, updates a probability map for EVE, manages battery and mission time, and stops with a final mission report.

## Run the project

1. Open walle-eve-signal-final.html in a modern browser.
2. Open sample-mission.json, copy its contents into the mission configuration box, then select Load Config.
3. Keep the hidden-state Debug view off for an honest autonomous run. It reveals EVE and clue locations.
4. Select Run for autonomous search or Step to advance one decision at a time. The live trace explains the selected action and belief update.
5. When the mission ends, review Final Mission Report and select Download trace + report. The downloaded JSON contains the exact config, action/observation/decision trace, belief peak after each action, and final report.

The original single-file project is preserved separately as walle-eve-signal-OG.html.

## Mission config

The configuration is JSON. The page includes a default config and validates user-provided values before starting.

- seed: deterministic world layout and sensor-noise seed.
- grid: width, height, and obstacleDensity.
- base: WALL-E's recharge location.
- battery: maximum charge and safe return reserve.
- costs: negative battery deltas for move and scan. Recharge consumes a mission action.
- signal: linear signal range and Gaussian noise standard deviation.
- mission.timeBudget: maximum number of successful actions before TIMEOUT.
- initialSignal: starting sensor reading. origin is its grid coordinate, strength is from 0 to 1, direction is an optional cardinal bearing toward EVE, and timestamp is metadata.
- clueCount: number of hidden clues.
- agent: decision interval and belief/scan planning parameters.

The directional prior weakly discounts candidate tiles behind the reported bearing. Clue locations remain hidden from the agent until WALL-E walks onto them. The live trace and exported observations omit the environment's private target and clue coordinates.

## Model and visualisation

The agent uses a classical probability-map planner. Signal readings update tile likelihoods; visiting a tile without finding EVE eliminates that candidate; discovered obstacles are marked impassable. The planner chooses targets from current belief, plans routes with known traversable tiles, and returns to base when battery reserve requires it.

The canvas shows the agent's belief as a blue-to-orange/red heatmap, the visited path, and the current planned route/target. Debug view is for inspection only and should remain off during a scored autonomous run.

## Suggested grading demonstration

Run the same seed with the sample initial signal and with a neutral starting signal (strength 0.5, direction null). Compare the exported action count, clues found, final confidence, battery use, and outcome. This makes the effect of prior evidence visible without changing the map seed. The exported traces provide the evidence for each decision; report comparative outcomes as observations from those runs, not as a general benchmark.

## Scope

The deliverable includes autonomous classical planning, noisy scans, clues, seeded missions, battery/time constraints, belief heatmap, action reasoning, and an exportable final report. Multi-agent coordination, spoofed signals, LLM planner comparison, broad random-seed benchmarking, and Gazebo/Webots physics are optional extensions and are not claimed as implemented.