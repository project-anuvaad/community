# Auditor


A Python package that provides standardized logging and error handling for the Anuvaad dataflow pipeline. This package serves features like session tracing, job tracing, error debugging, and troubleshooting.

## Installation

**Prerequisites:**
- Python 3.7

**Source code:** [GitHub Repository](https://github.com/project-anuvaad/anuvaad-em)

**Command:**

```bash
pip install anuvaad-auditor==0.1.6
```

## Logging/Auditing

This part of the library provides features for logging by exposing the following functions:

**Import file:**

```python
from anuvaad_auditor import loghandler
```

### Functions

#### log_info

Logs INFO level information.

```python
loghandler.log_info(<str(message)>, <json(input_object)>)
```

#### log_debug

Logs DEBUG level information.

```python
loghandler.log_debug(<str(message)>, <json(input_object)>)
```

#### log_error

Logs ERROR level information. Should be used for logical errors like “File is not valid”, “File format not accepted” etc.

```python
loghandler.log_error(<str(error_message)>, <json(input_object)>, <exception_object>)
```

#### log_exception

Logs EXCEPTION level information. Should be used in case of exceptions like “TypeError”, “KeyError” etc.

```python
loghandler.log_exception(<str(exception_message)>, <json(input_object)>, <exception_object>)
```

### Notes

- In all the functions, message and input-object are **mandatory**.
- These functions build an object using these parameters and index them to Elasticsearch for easy tracing.
- Ensure all major functions have a `log_info` call, all exceptions have `log_exception` calls, and all logical errors have `log_error` calls.

## Error Handling

This part of the library provides features for standardizing and indexing the error objects of the pipeline.

**Import file:**

```python
from anuvaad_auditor import errorhandler
```

### Functions

#### post_error

Returns a standard error object for replying back to the client during a SYNC call and indexes the error to an error index.

```python
errorhandler.post_error(<str(error_code)>, <str(error_msg)>, <exception_object>)
```

#### post_error_wf

Constructs a standard error object which will be indexed to a different error index and PUSHES THE ERROR TO WFM internally.

```python
errorhandler.post_error_wf(<str(error_code)>, <str(error_msg)>, <json(input_object)>, <exception_object>)
```

**Usage Notes:**

- Use `post_error_wf` for flows triggered via Kafka or REST through WFM.
- Ensure both log functions and error functions are used in case of exceptions or errors.
- Errors are indexed to two different indexes: Error index and Audit Index.
- Use `post_error_wf` carefully, as this method will take the entire job to FAILED state.

### Example Usage

```python
from anuvaad_auditor.loghandler import log_info, log_debug, log_error, log_exception
from anuvaad_auditor.errorhandler import post_error, post_error_wf

def example_function():
    input_object = {"example": "data"}
    try:
        log_info("Starting example function", input_object)
        # Your code logic here
        raise KeyError('A sample key error')
    except KeyError as ke:
        log_exception("Caught a KeyError", input_object, ke)
        error_obj = post_error("KEY_ERROR", "KeyError occurred in example function", ke)
        return error_obj
    except Exception as e:
        log_exception("An unexpected error occurred", input_object, e)
        error_obj = post_error_wf("UNEXPECTED_ERROR", "Unexpected error in example function", input_object, e)
        return error_obj

example_function()
```

### Current Version

`anuvaad-auditor==0.1.1` - Please use this version.
