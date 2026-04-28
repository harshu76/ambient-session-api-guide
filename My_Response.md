# My Response — Session Completion: Webhooks vs. Polling

## What’s the Issue?

Right now, we’re using polling to check when a session is complete — meaning the system keeps asking the server if processing is done.

A more efficient approach is webhooks, where the server notifies us automatically as soon as the session is complete.

Webhook support already exists in the backend, but it isn’t documented or exposed yet.

---

## Both Perspectives Are Valid

- **Developer concern:** Polling doesn’t scale well and can delay downstream systems. Integrators are already asking for more real-time updates.
- **PM concern:** We’re close to a UI release, and shifting focus now could introduce delays without strong customer pressure.

This isn’t urgent today, but it’s likely to become a bottleneck as usage grows.

---

## Suggested Path Forward

1. Ship the UI release as planned (no change)
2. Document webhook support (mark as Beta)
3. Test with select partners (2–4 weeks post-launch)
4. Roll out broadly once stable
5. Deprecate polling after validation

---

## Recommendation

A practical next step would be to document the existing webhook support. This is low effort, doesn’t require engineering changes, and helps us move toward a more scalable solution without impacting the current release.
