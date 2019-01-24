# unittest_additions

Helpful extras built to make python unittesting easier.

## Features

* Additional Asserts
* Temporary Files
* `mock_open` Line Iteration

## Additional Asserts

| Method                  | Checks that ...
|-------------------------|----------------
| `assertIsEmpty(c)`      | `len(c) == 0`
| `assertIsNotEmpty(c)`   | `len(c) > 0`

## Example Uses

Below are some example uses for some of the features.

### Additional Asserts

To use the additional asserts, add the mixin to your `TestCase` class.

```python
class MixedTestCase(TestCase, AdditionalAssertsMixin):
    def test_a_test:
        self.assertIsEmpty([])
        self.assertIsNotEmpty(('hello',))
```

### Temporary Files

```python
def test_using_temp_file(self):
    with TempFile(TEST_FILE, TEST_DATA) as tf:
    	# File TEST_FILE now exists with TEST_DATA inside.        
        tf.append(MORE_TEST_DATA)
        # File TEST_FILE now has TEST_DATA with MORE_TEST_DATA appended.
    
    # File TEST_FILE no longer exists.
```

### mock_open Line Iteration

When mocking a file using `mock_open`, "code-under-test" using line iteration (as in the example below) will not work as expected. (`do_something` will not be called.)

```python
def my_function(fn):
    with open(fn, 'r') as f:
        for l in f:
    	    do_something(l)
```

To enable line iteration you can use `add_line_iter_to_mock_open`.

```python
    @patch('builtins.open', new_callable=mock_open, read_data='line 0\nline 1\n')
    def test_function(self, open_mock):

        add_line_iter_to_mock_open(open_mock)

        # Line iteration over the mock_open read_data will now work.
```