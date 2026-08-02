========================================================================
BUILD 4 — PRE-REGISTERED SINGLE LOOK
========================================================================
stratum: post_changepoint   changepoint: 2026-07-13T02:40:01.064525+00:00
trigger: 604/600 trigger-COMPLETE   [the N=600 gate — MET]
analyzable cohort: 249 after attrition  (survivors 12 / dead 238)   fingerprint 197054c20180ef5f0d3efd15
attrition: {'missing_x_no_window': 0, 'missing_x_no_depth': 0, 'missing_x_no_15m': 0, 'missing_y_indeterminate': 186, 'missing_y_death_ts_null': 0, 'missing_y_event_time_ambiguous': 281, 'malformed_death_ts': 0, 'band_excluded': 220, 'scope_never_cleared': 1, 'scope_hard_fail': 3, 'scope_landmark_pre15m_death': 73, 'scope_phantom': 0}

PRIMARY (v13-runway-landmark)  15m score → runway from the 15m landmark
  Somers' D = -0.0208   C-index = 0.4896   p = 0.58744  (10000 within-block permutations, seed 20260716)
  VERDICT CLASS: NULL (ROBUST / NOT_POINT_IDENTIFIED / ADVERSE / NULL — v6 §10 mapping, bounds-aware)

SECONDARIES (Holm-corrected family of 3):
  S1_1h_landmark: D = +0.0227  p = 0.61764  (ns after Holm)
  S2_depth_strat: D = -0.0080  p = 0.83052  (ns after Holm)
  S3_peak_doomed: D = +0.1681  p = 0.00010  ✦ significant after Holm

DESCRIPTIVE PANELS (pre-registered, point estimates only — no α):
  per-stratum pre_changepoint: n=118 (of frame 488) events=0  D = n/a
    ⚠ NON-CONFIRMATORY — descriptive only; reflects unrepaired historical missing-Y (the 06-26→07-06 SOL-ref blackout); cannot override the POST primary verdict.
  per-stratum straddle: n=0 (of frame 295) events=0  D = n/a
    ⚠ NON-CONFIRMATORY — descriptive only; reflects unrepaired historical missing-Y (the 06-26→07-06 SOL-ref blackout); cannot override the POST primary verdict.
  initial-pool NULL_pre_phase_d: n=249  D = -0.0208
    (initial_pool_label is unstamped pre-Phase-D — the single NULL bucket IS the primary cohort; the contrast becomes informative when Phase D stamps it)
  NC_F-as-T=0: D = -0.0207  (+1 NC_F as T=0 events, 0 unscorable, n=250; tied at 0 ⇒ mutually non-comparable, comparable vs all later events)

G1  events(deaths)=237  censored(survivors)=12  comparable pairs=30734; detectable HR per SD ≈ 1.200
G3  runway Manski bounds  C [0.075, 0.922]  D [-0.850, 0.844]  over 386 unanalyzed
    attrition: {'missing_x_no_window': 0, 'missing_x_no_depth': 0, 'missing_x_no_15m': 0, 'missing_y_indeterminate': 186, 'missing_y_death_ts_null': 0, 'missing_y_event_time_ambiguous': 281, 'malformed_death_ts': 0, 'band_excluded': 220, 'scope_never_cleared': 1, 'scope_hard_fail': 3, 'scope_landmark_pre15m_death': 73, 'scope_phantom': 0}
G4  no new analysis, subgroup, or threshold may be introduced in response to these gates
