---
layout: post
title: "(Trivial) Interaction with EC2 instances"
date: 2017-12-27 12:00:00 -0500
categories: code
permalink: /brews/:title
---

Recently, I have been spending a little more time interacting with my EC2 instances.
This led me to read more about the excellent [`boto3`](http://boto3.readthedocs.io/en/latest/) Python module.

`boto3` offers a lot of functionality to manage your AWS resources in a programmatic way.
In particular, if you are just getting started with `boto3`, you may find it useful to read more about high-level interaction via `boto3.resource`s and low-level interaction via `boto3.client`s.

This post is not about the details of `boto3`, although I encourage you (and myself) to read more about it if you plan to use AWS.
Instead, this post is about sharing some embarassingly simple code that I wrote for myself to further simplify trivial operations such as:

1. getting the current state of all my instances
2. start an instance
3. get the public DNS of an instance
4. stop an instance.

All the code discussed in this post is available in [this repository](https://github.com/mattiaciollaro/boto3_util).

Here is the back story.

I get to work in the morning and I start an interactive Python session on my MacBook.
I am really sleepy and I want to remind to myself which instances are available to my `mattia-admin` profile (more on profiles later).
So, I want to be able to simply type

```python
from boto3_util import EC2

ec2 = EC2('mattia-admin')

ec2.get_all_states()
```

and read

```
{
    'instance1_id': {'Code': 80, 'Name': 'stopped'},
    'instance2_id': {'Code': 80, 'Name': 'stopped'}
}
```

*"Great. Fortunately, yesterday I remembered to shut down both of my EC2 instances!"*

I also want to be able to write

```python
ec2.start_instance('instance1_id')
```

and check with

```python
ec2.get_instance_state('instance1_id')
```

that the instance is indeed starting.
On my screen, I should then read:

```python
{
    'Code': 0,
    'Name': 'pending'
}
```

*"Come on... I'm already late today!"*

Frenetic calls to `ec2.get_instance_state('instance1_id')` will eventually yield:

```python
{
    'Code': 16,
    'Name': 'running'
}

```

*"Finally! Now I just need the instance public DNS to ssh into it!"*

Now, I want to be able to type

```python
ec2.get_instance_public_dns('instance1_id')
```

and get

```python
'ec2-<some_ip>.compute-1.amazonaws.com'
```

I ssh into `instance1` with something like

```bash
ssh -i <path_to_my_pem_file> ec2-user@ec2-<some_ip>.compute-1.amazonaws.com
```

Fast-forward 8 hours:

*"Yeah, I'm done for today! Time to go home and watch Netflix! But first, I need to do one last thing..."*

```python
ec2.stop_instance('instance1_id')
```

I close the MacBook and go home.
Once home: *"Oh my... did I also start instance2 at some point today and forget to shut it down?"*

```python
ec2.get_all_states()
```

```
{
    'instance1_id': {'Code': 80, 'Name': 'stopped'},
    'instance2_id': {'Code': 16, 'Name': 'running'}
}
```

*"Crap. I knew it!"*

```python
ec2.stop_instance('instance2_id')
```

*"Netflix, here I coooooome..."*, and I'm on the couch.

Note that the same interaction using `boto3` would have been very similar, but a little more cumbersome.
This is the only reason why I wrote these two or three utilities.
Because I am lazy.

The only parameters in all of the code that we discussed are the id of the instance you are interested in (in methods such as `start_instance`, `stop_instance`, `get_instance_state`, and `get_instance_public_dns`) and the AWS profile you are using.
Throughout this example, I am using a profile named `mattia-admin`.

`mattia-admin` is defined as follows in my `~/.aws/credentials` and `~/.aws/config` files.

- `credentials`:
```
[mattia-admin]
aws_secret_access_key = <my_aws_secret_access_key>
aws_access_key_id = <my_secret_access_key>
```

- `config`
```
[mattia-admin]
region = us-east-1
output = json
```

Not that this is too interesting, but my point is that this is the information which is passed to the `ec2` handle that we created with

```python
ec2 = EC2('mattia-admin')
```

in the very first block.

If you don't specify a profile name,

```python
ec2 = EC2()
# equivalent to ec2 = EC2('default')
```

assumes that you want to use your `default` AWS profile.

More functionality may be added in the future.
But, really, all this is just further simplifying trivial operations that you can already perform using `boto3`.

If you have comments, thoughts, or any suggestions, please do feel free to reach out.