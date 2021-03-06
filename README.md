python-junit-xml
================

[![chadhutchins182](https://circleci.com/gh/chadhutchins182/python-junit-xml.svg?style=shield)](https://app.circleci.com/pipelines/github/chadhutchins182/python-junit-xml)

About
-----

A Python module for creating JUnit XML test result documents that can be
read by tools such as Atlassian Bamboo or Jenkins.

Code originally forked from: <https://github.com/kyrus/python-junit-xml>
read by tools such as Jenkins or Bamboo. If you are ever working with test tool or
test suite written in Python and want to take advantage of Jenkins' or Bamboo's
pretty graphs and test reporting capabilities, this module will let you
generate the XML test reports.

*As there is no definitive Jenkins JUnit XSD that I could find, the XML
documents created by this module support a schema based on Google
searches and the Jenkins JUnit XML reader source code. File a bug if
something doesn't work like you expect it to.
For Bamboo situation is the same.*

Installation
------------

Install using pip or easy_install:

```bash
 pip install junit-xml
 or
 easy_install junit-xml
```

You can also clone the Git repository from Github and install it manually:

```bash
    git clone https://github.com/kyrus/python-junit-xml.git
    python setup.py install
```

Using
-----

Create a test suite, add a test case, and print it to the screen:

```python
    from junit_xml import TestSuite, TestCase

    test_cases = [TestCase('Test1', 'some.class.name', 123.345, 'I am stdout!', 'I am stderr!')]
    ts = TestSuite("my test suite", test_cases)
    # pretty printing is on by default but can be disabled using prettyprint=False
    print(TestSuite.to_xml_string([ts]))
```

Produces the following output

```xml
    <?xml version="1.0" ?>
    <testsuites>
        <testsuite errors="0" failures="0" name="my test suite" tests="1">
            <testcase classname="some.class.name" name="Test1" time="123.345000">
                <system-out>
                    I am stdout!
                </system-out>
                <system-err>
                    I am stderr!
                </system-err>
            </testcase>
        </testsuite>
    </testsuites>
```

Writing XML to a file:

```python
    # you can also write the XML to a file and not pretty print it
    with open('output.xml', 'w') as f:
        TestSuite.to_file(f, [ts], prettyprint=False)

```

Additional *TestCase* functions

```python
    def add_stdout(self, stdout=None):
        """Adds standard out to the test case"""

    def add_stderr(self, stderr=None):
        """Adds standard error to the test case"""

    def add_error_info(self, message=None, output=None, error_type=None):
        """Adds an error message, output, or both to the test case"""

    def add_failure_info(self, message=None, output=None, failure_type=None):
        """Adds a failure message, output, or both to the test case"""

    def add_skipped_info(self, message=None, output=None):
        """Adds a skipped message, output, or both to the test case"""

    def add_timestamp(self, timestamp=None):
        """Adds timestamp to the test case. Time is in seconds"""
```

See the unit tests for more examples.

NOTE: Unicode characters identified as "illegal or discouraged" are automatically
stripped from the XML string or file.

Running the tests
-----------------

```bash
    # activate your virtualenv
        pip install coverage
        pip install pytest
        pip install six
        coverage run -m pytest tests
```
