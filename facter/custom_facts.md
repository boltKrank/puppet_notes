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

###export FACTER_

Just running export FACTER_<fact_name>="fact_value"

Will add the fact. NOTE: it will disappear when you restart the session (unless you put it in bashrc or similar)

### $LOAD_PATH

If you have a /facter directory in any of the directories listed by  `ruby -e 'puts $LOAD_PATH'` facter will pick them up and search for Ruby files within. In that Ruby file, you can put something like:

```
Facter.add('<fact_name>') do
  setcode "<fact_value>"
end
```

When you can add a command to replace the <fact_value> part and use this command to get the information you need.


###FACTERLIB

If you add a directory to the FACTERLIB environment variable, Facter will look there too.

'i.e. export FACTERLIB=$FACTERLIB:/my/new/directory '

NOTE: It won't search recursively.

###Pluginsync (depricated in 2.4, restored in 3.0.2)
