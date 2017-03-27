# puppet_profile
Information on profiling puppet

### Single Puppet run puppet profile:
On the agent, just run
```
  puppet agent -t --profile
```

This will add the profile data to the logs on the master.

### For all puppet runs:
In puppet.conf on the master, add:
```
  profile=true
```

_NOTE: This will increase the log size substantially
