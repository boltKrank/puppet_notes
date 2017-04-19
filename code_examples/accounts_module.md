# Full test example of puppetlabs/accounts

### Step 1. Download module from forge

On your puppet master you can install the module using the following:

```

puppet module install puppetlabs/accounts

```

Or if you wish to test it using `puppet apply` you can install it on a node machine.

### Step 2. Create test module

This is the module that's going to be calling/using stuff from the puppetlabs/accounts module.

In this situation, I'll call the module "accounts_test".

```

puppet module generate simon-accounts_test

```

### Step 3. Call the accounts class

In the init.pp file (default starting file) I've made a call to make an example user named "example"

Contents of the init.pp file:


```


```
