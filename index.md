---
title: Notes to self (and anyone else)
---

A collection of notes and coding tips

## Python Mocks
When attempting to `@patch` using unittest mocks to define the target for a chain of calls that may also occur across multiple lines e.g.
```
    ...
    response = more.modules.some_function(param='value', other_param='other_value')
    value_object = response.some_list[0].some_value
    outputs = []
    if value_object.value:
        parsed_value = value_object.value.value_dict()
        return parsed_value.get('outputs', [])
    else:
        return value_object.other_value
    ...
```
Define the target call behaviours:
```
@patch(target="some.module.Type")
def test_ml_inference(the_mock):
    the_mock.more.modules.some_function.return_value.some_list[0].some_value.value.value_dict.return_value = {
        'outputs': [{'index': 1, 'real_value': 'the_value'}]
    }
    the_mock.more.modules.some_function.return_value.some_list[0].some_value.other_value = 'mocked value'
```

## SSH Agent Forwarding
When needing to access git repositories on remote hosts, forwarding prevents the need to copy over a private key each time, just `ssh-add ~/.ssh/the_private_key_file` and adding to `.ssh/config`:
```
Host remost_host_ip_address_or_hostname
    ForwardAgent yes
```
