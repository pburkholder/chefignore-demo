This cookbook demonstrates that chef-client 12.0.3 does not honor the contents
of `chefignore`, which is a security bug since content the creator considers
secret is uploaded to the chef-server in contravention to what one expects:


The `secrets` file in this cookbook has one secret:

    MySecretPassword

and the `chefignore` file has one entry:

    secrets

In chef-client 11.X, the `knife cookbook upload` command would not upload the
secrets file per the contents of chefignore, however, observe what happens

```
> knife --version
Chef: 12.0.3
> knife cookbook list | grep chefignore
> pwd
/Users/pburkholder/Hack/chefignore-demo
> knife cookbook upload -o ~/Hack chefignore-demo
Uploading chefignore-demo [0.1.0]
Uploaded 1 cookbook.
> cd ~/tmp
> knife cookbook download chefignore-demo
Downloading chefignore-demo cookbook version 0.1.0
Downloading resources
Downloading providers
Downloading recipes
Downloading definitions
Downloading libraries
Downloading attributes
Downloading files
Downloading templates
Downloading root_files
Cookbook downloaded to /Users/pburkholder/tmp/chefignore-demo-0.1.0
> more chefignore-demo-0.1.0/secrets
MySecretPassword
> more chefignore-demo-0.1.0/chefignore
secrets
>
```

Oops, so we've uploaded the secrets, and downloaded them, despite the
instructions of the `chefignore` file.
