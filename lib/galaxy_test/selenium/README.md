[Selenium](https://selenium-python.readthedocs.io/) is performing full-stack Galaxy testing. 
All tests are implemented using Python and running using [nosetests](https://nose.readthedocs.io/en/latest/).
We replicate desired Galaxy behavior by parsing [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
using XPath or CSS selectors
 
### Quick start
Run Selenium testing
```
./run_tests.sh -selenium
```

Run specific selenium test 
```
./run_tests.sh -selenium lib/galaxy_test/selenium/test_history_multi_view.py
```


## Development tips

#### nosetests
Obviously, it's quite tedious to always run ```./run_tests.sh -selenium ``` while implementing new great test. 
Thus it's highly advised to use ```nosetests``` against already running galaxy server.  
To make it work, the developer will need to start galaxy first:

```
GALAXY_CLIENT_DEV_SERVER=1 sh run.sh
```

Watch client update (if you are adding new ids or selectors)
```
 make client-dev-server
```

Specify server address
```
 export GALAXY_TEST_EXTERNAL=http://localhost:8080/
 ```

Activate virtualenv
```
. .venv/bin/activate
```
Finally, run selenium test!
```
nosetests lib/galaxy_test/selenium/test_workflow_editor.py:WorkflowEditorTestCase.test_data_input
```

You can add ```-s``` to see output, if test is not failing.

#### Navigation
Preferably, we try to avoid hard-coding selectors and make it recyclable. Please add new selectors in [navigation.yml](https://github.com/galaxyproject/galaxy/blob/dev/lib/galaxy/selenium/navigation.yml), 
so we can reuse them in future. It will create new [Smart Component](https://github.com/galaxyproject/galaxy/blob/dev/lib/galaxy/selenium/smart_components.py),
which can be accessed with ```self.components.{{selector_name}}```

#### Visualization
We definitely want see how our test progresses and whether it pressed the right button.
###### Screenshots

This requires GALAXY_TEST_ERRORS_DIRECTORY directory to be defined. Could be used just as:
```
GALAXY_TEST_ERRORS_DIRECTORY=~/galaxy/test-dir nosetests lib/galaxy_test/selenium/test_workflow_editor.py
```
###### Non-headless

You can run selenium test just in your browser. Please install https://github.com/mozilla/geckodriver or 
https://sites.google.com/a/chromium.org/chromedriver/. Then run:
```
GALAXY_TEST_SELENIUM_HEADLESS=0 ./run_tests.sh
```

or 

```
GALAXY_TEST_SELENIUM_HEADLESS=0 nosetests lib/galaxy_test/selenium/test_workflow_editor.py:WorkflowEditorTestCase.test_data_input
```

