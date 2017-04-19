# Custom facts:

## What
Custom facts are facts that aren't included in the initial install of facter

## Why
When you need to get info or use info to differentiate a node and this information isn't available in the out of the box solution.

Common examples may be:

- Region
- Data center
- Business unit
- Rack address (for physical servers)


## How

Some ways of adding custom facts:
- Using the “export FACTER_…” syntax.
- via the $LOAD_PATH setting
- FACTERLIB
- Pluginsync (distribute facts through modules)
