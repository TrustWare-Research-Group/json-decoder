# json-decoder
A simple JSON decoder for stringified JSON.

Check out the documentation [here](https://docs.python.org/3/library/json.html). Original source code is available [here](https://github.com/python/cpython/blob/3.14/Lib/json/__init__.py).

## Project structure
```markdown
json-decoder
├── bin
└── README.md
```
- **bin**: This directory contains the binary subject to run the fuzzer on.

## Input structure
The following materials describe the standard specifications for each supported format. Use them as a reference to understand what valid input should look like:
* `JSON`: [RFC 8259](https://datatracker.ietf.org/doc/html/rfc8259)

Inputs in the below structure are accepted for the following formats:

* `'["foo", {"bar":["baz", null, 1.0, 2]}]'`
* `'{"__complex__": true, "real": 1, "imag": 2}'`
* `'{"json":"obj"}'`

## Output structure

Without bugs
```bash
$ json_decoder --str-json '{"a":1, "b":2}'
Output decoded data: {'a': 1, 'b': 2} of type <class 'dict'>
No bugs found. Skipping CSV creation
Saved bug count report and tracebacks for the bugs encountered!
Final bug count: defaultdict(<class 'int'>, {})
```

With bugs
```bash
$ json_decoder --str-json '{"a":'
An invalidity bug has been triggered: Expecting value: line 1 column 6 (char 5)
Traceback (most recent call last):
  File "json_decoder_stv.py", line 258, in <module>
    data = loads(args.str_json)
          ^^^^^^^^^^^^^^^^^^^^
  File "buggy_json/__init__.py", line 134, in loads
    return _default_decoder.decode(s)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "buggy_json/decoder_stv.py", line 407, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "buggy_json/decoder_stv.py", line 423, in raw_decode
    obj, end = self.scan_once(s, idx)
               ^^^^^^^^^^^^^^^^^^^^^^
  File "buggy_json/scanner_stv.py", line 67, in scan_once
    return _scan_once(string, idx)
           ^^^^^^^^^^^^^^^^^^^^^^^
  File "buggy_json/scanner_stv.py", line 37, in _scan_once
    return parse_object((string, idx + 1), strict,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "buggy_json/decoder_stv.py", line 251, in JSONObject
    raise JSONDecodeError("Expecting value", s, err.value) from None
buggy_json.decoder_stv.JSONDecodeError: Expecting value: line 1 column 6 (char 5)
Saved bug count report and tracebacks for the bugs encountered!
Final bug count: defaultdict(<class 'int'>, {('invalidity', <class 'buggy_json.decoder_stv.JSONDecodeError'>, 'Expecting value: line 1 column 6 (char 5)', 'buggy_json/decoder_stv.py', 251): 1})
```

## Setup instructions
<!-- Make sure to have a chosen linux environment setup to run the binary scripts. -->
Use the respective binary script for your OS.

## Instructions to run
Run the binary file in the terminal as shown:
```bash
$ json-decoder [-h] [--str-json STR_JSON]
```
Consult the documentation with the `--help` flag if you are in doubt.

## List of removed functions
* `load`
* `dump`
* `dumps`
