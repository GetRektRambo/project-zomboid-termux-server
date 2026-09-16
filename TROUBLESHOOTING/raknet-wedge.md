# RakNet Wedge + the Dog War (War 3)

## The wedge

Every FAILED client join can wedge the RakNet listener: afterward it
accepts nothing, while looking completely alive on the outside. Ports
bound, process running, zero serves.

**Protocol:** probe PONG; restart on fail; NEVER retry a wedged server.
It's already dead — a restart is mercy, not debugging.

## The dog war

The related war: a health monitor whose probe is silently broken
against your server build will happily restart a HEALTHY, POPULATED
server on every fail-limit strike — manufacturing the very disconnect
pattern you're hunting.

**Symptom signature:** clients demonstrably connect while your probe
reports dead. If reality disagrees with your monitor, **the monitor
is the suspect.**

**Resolution:** demote probes to warn-only; make **process death the
sole restart trigger**. Validate probes against known-good state after
every rebuild, not just when written.

## The lesson generalized

Any component whose only job is reacting to another component's
report needs that report validated against ground truth — one time,
to prove the wiring, before trusting it forever. An unvalidated
monitor is a liability wearing a reliability costume.
