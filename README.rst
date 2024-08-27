Scott's Changes
----------------

1. Add "Skipped" counts to testsuite summary

My Example  
----------

.. code-block:: python

    from junit_xml import TestSuite, TestCase, to_xml_report_string

    def junit_tester():
        test_cases = [TestCase('Test1', 'some.class.name', 123.345),
                      TestCase('Test2', 'some.class.name', 123.345),
                      TestCase('Test3', 'some.class.name', 123.345),
                      TestCase('Test4', 'some.class.name', 123.345)]
        test_cases[1].add_error_info("It's a error!")
        test_cases[2].add_failure_info("I am a failure!")
        test_cases[3].add_skipped_info("Just skipped for nothing!")
        ts = TestSuite("my test suite", test_cases)
        # pretty printing is on by default but can be disabled using prettyprint=False
        print(to_xml_report_string([ts]))

    if __name__ == '__main__':
        junit_tester()

My Output  
----------

.. code-block:: xml

        <?xml version="1.0" ?>
        <testsuites disabled="0" errors="1" failures="1" tests="4" skipped="1" time="493.38">
	        <testsuite disabled="0" errors="1" failures="1" name="my test suite" skipped="1" tests="4" time="493.38">
		        <testcase name="Test1" time="123.345000" classname="some.class.name"/>
		        <testcase name="Test2" time="123.345000" classname="some.class.name">
			        <error type="error" message="It's a error!"/>
        		</testcase>
	        	<testcase name="Test3" time="123.345000" classname="some.class.name">
		        	<failure type="failure" message="I am a failure!"/>
        		</testcase>
        		<testcase name="Test4" time="123.345000" classname="some.class.name">
	        		<skipped type="skipped" message="Just skipped for nothing!"/>
		        </testcase>
	        </testsuite>
        </testsuites>

Original Content
================

python-junit-xml
================
.. image:: https://travis-ci.org/kyrus/python-junit-xml.png?branch=master

About
-----

A Python module for creating JUnit XML test result documents that can be
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

::

	pip install junit-xml
	or
	easy_install junit-xml

You can also clone the Git repository from Github and install it manually:

::

    git clone https://github.com/kyrus/python-junit-xml.git
    python setup.py install

Using
-----

Create a test suite, add a test case, and print it to the screen:

.. code-block:: python

    from junit_xml import TestSuite, TestCase

    test_cases = [TestCase('Test1', 'some.class.name', 123.345, 'I am stdout!', 'I am stderr!')]
    ts = TestSuite("my test suite", test_cases)
    # pretty printing is on by default but can be disabled using prettyprint=False
    print(TestSuite.to_xml_string([ts]))

Produces the following output

.. code-block:: xml

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

Writing XML to a file:

.. code-block:: python

    # you can also write the XML to a file and not pretty print it
    with open('output.xml', 'w') as f:
        TestSuite.to_file(f, [ts], prettyprint=False)

See the docs and unit tests for more examples.

NOTE: Unicode characters identified as "illegal or discouraged" are automatically
stripped from the XML string or file.

Running the tests
-----------------

::

    # activate your virtualenv
    pip install tox
    tox

Releasing a new version
-----------------------

1. Bump version in `setup.py`
2. Build distribution with `python setup.py sdist bdist_wheel`
3. Upload to Pypi with `twine upload dist/*`
4. Verify the new version was uploaded at https://pypi.org/project/junit-xml/#history
