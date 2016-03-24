All available settings in test data
===================================

.. contents::
   :depth: 2
   :local:

Setting table
-------------

..
   The Setting table is used to import test libraries, resource files and
   variable files and to define metadata for test suites and test
   cases. It can be included in test case files and resource files. Note
   that in a resource file, a Setting table can only include settings for
   importing libraries, resources, and variables.

테이블은 설정하는 것은 테스트 라이브러리, 리소스 파일, 변수 파일을
가져오거나, test suite, test case를 위한 메타 데이터를 지정하기 위해
쓰인다. 이것은 test case 파일이나 리소스 파일에 포함될 수 있다. 리소스
파일의 경우 설정 테이블은 가져올 라이브러리, 리소스, 변수를 위한
설정값만을 포함할 수 있다.

..
   .. table:: Settings available in the Setting table
      :class: tabular

      +-----------------+--------------------------------------------------------+
      |       Name      |                         Description                    |
      +=================+========================================================+
      | Library         | Used for `importing libraries`_.                       |
      +-----------------+--------------------------------------------------------+
      | Resource        | Used for `taking resource files into use`_.            |
      +-----------------+--------------------------------------------------------+
      | Variables       | Used for `taking variable files into use`_.            |
      +-----------------+--------------------------------------------------------+
      | Documentation   | Used for specifying a `test suite`__ or                |
      |                 | `resource file`__ documentation.                       |
      +-----------------+--------------------------------------------------------+
      | Metadata        | Used for setting `free test suite metadata`_.          |
      +-----------------+--------------------------------------------------------+
      | Suite Setup     | Used for specifying the `suite setup`_.                |
      +-----------------+--------------------------------------------------------+
      | Suite Teardown  | Used for specifying the `suite teardown`_.             |
      +-----------------+--------------------------------------------------------+
      | Force Tags      | Used for specifying forced values for tags when        |
      |                 | `tagging test cases`_.                                 |
      +-----------------+--------------------------------------------------------+
      | Default Tags    | Used for specifying default values for tags when       |
      |                 | `tagging test cases`_.                                 |
      +-----------------+--------------------------------------------------------+
      | Test Setup      | Used for specifying a default `test setup`_.           |
      +-----------------+--------------------------------------------------------+
      | Test Teardown   | Used for specifying a default `test teardown`_.        |
      +-----------------+--------------------------------------------------------+
      | Test Template   | Used for specifying a default `template keyword`_      |
      |                 | for test cases.                                        |
      +-----------------+--------------------------------------------------------+
      | Test Timeout    | Used for specifying a default `test case timeout`_.    |
      +-----------------+--------------------------------------------------------+



.. table:: 설정 테이블에서 가능한 설정값
   :class: tabular

   +-----------------+--------------------------------------------------------+
   |       Name      |                         Description                    |
   +=================+========================================================+
   | 라이브러리      | `라이브러리를 가져온다 <importing libraries_>`__       |
   +-----------------+--------------------------------------------------------+
   | 리소스          | `리소스 파일을 가져온다 <taking resource files         |
   |                 | into use_>`__ .                                        |
   +-----------------+--------------------------------------------------------+
   | 변수            | `변수 파일을 가져온다. <taking variable                |
   |                 | files into use_>`__ .                                  |
   +-----------------+--------------------------------------------------------+
   | 문서            | `test suite`__ 나 `resource file`__  을 문서를         |
   |                 | 지정한다.                                              |
   +-----------------+--------------------------------------------------------+
   | 메타 데이타     | `free test suite metadata`_ 를 설정하는데 쓰인다.      |
   +-----------------+--------------------------------------------------------+
   | Suite Setup     | `suite setup`_ 을 지정하는데 쓴다.                     |
   +-----------------+--------------------------------------------------------+
   | Suite Teardown  | `suite teardown`_ 을 지정하는데 쓴다.                  |
   +-----------------+--------------------------------------------------------+
   | Force 태그      | `tagging test cases`_ 할때,                            |
   |                 | 태그를 위한 강제로 값을 지정한다.                      |
   +-----------------+--------------------------------------------------------+
   | Default 태그    | `tagging test cases`_ 할때,                            |
   |                 | 태그를 위한 기본값을 지정한다.                         |
   +-----------------+--------------------------------------------------------+
   | Test Setup      | `test setup`_ 을 지정한다.                             |
   +-----------------+--------------------------------------------------------+
   | Test Teardown   | 기본 `test teardown`_ 을 지정한다.                     |
   +-----------------+--------------------------------------------------------+
   | Test Template   | test case를 위한 기본 `template keyword`_ 을 지정한다. |
   +-----------------+--------------------------------------------------------+
   | Test Timeout    | 기본  `test case timeout`_ 을 지정한다.                |
   +-----------------+--------------------------------------------------------+

.. note:: 예를 들어, :setting:`Documentation:` 처럼 모든 설정 이름은
           선택적으로 맨끝에 colon를 포함한다. 일반 텍스트 형식을 쓰는
           것이 설정값을 읽는 것을 편하게 만든다.

__ `Test suite documentation`_
__ `Documenting resource files`_


Test Case table
---------------

..
   The settings in the Test Case table are always specific to the test
   case for which they are defined. Some of these settings override the
   default values defined in the Settings table.

Test case 에 설정한 값은 항상 test case를 위해 정의된 특정한 값이다.
이러한 설정 중 일부는 설정 테이블에 정의된 기본값을 대체한다.

..
   .. table:: Settings available in the Test Case table
      :class: tabular

      +-----------------+--------------------------------------------------------+
      |      Name       |                         Description                    |
      +=================+========================================================+
      | [Documentation] | Used for specifying a `test case documentation`_.      |
      +-----------------+--------------------------------------------------------+
      | [Tags]          | Used for `tagging test cases`_.                        |
      +-----------------+--------------------------------------------------------+
      | [Setup]         | Used for specifying a `test setup`_.                   |
      +-----------------+--------------------------------------------------------+
      | [Teardown]      | Used for specifying a `test teardown`_.                |
      +-----------------+--------------------------------------------------------+
      | [Template]      | Used for specifying a `template keyword`_.             |
      +-----------------+--------------------------------------------------------+
      | [Timeout]       | Used for specifying a `test case timeout`_.            |
      +-----------------+--------------------------------------------------------+

.. table:: Test Case table에서 가능한 설정값
   :class: tabular

   +-----------------+--------------------------------------------------------+
   |      Name       |                         Description                    |
   +=================+========================================================+
   | [Documentation] | `test case documentation`_ 을 지정하는데 쓴다.         |
   +-----------------+--------------------------------------------------------+
   | [Tags]          | `test case를 태깅 <tagging test cases_>`__ 한다.       |
   +-----------------+--------------------------------------------------------+
   | [Setup]         | `test setup`_ 을 지정한다.                             |
   +-----------------+--------------------------------------------------------+
   | [Teardown]      | `test teardown`_ 을 지정한다.                          |
   +-----------------+--------------------------------------------------------+
   | [Template]      | `template keyword`_ 을 지정한다.                       |
   +-----------------+--------------------------------------------------------+
   | [Timeout]       | `test case timeout`_ 을 지정한다.                      |
   +-----------------+--------------------------------------------------------+



Keyword table
-------------

..
   Settings in the Keyword table are specific to the user keyword for
   which they are defined.

키워드 테이블의 설정값은 user keyword을 위해 정의된 특별한 값이다.

..
   .. table:: Settings available in the Keyword table
      :class: tabular

      +-----------------+--------------------------------------------------------+
      |      Name       |                         Description                    |
      +=================+========================================================+
      | [Documentation] | Used for specifying a `user keyword documentation`_.   |
      +-----------------+--------------------------------------------------------+
      | [Tags]          | Used for specifying `user keyword tags`_.              |
      +-----------------+--------------------------------------------------------+
      | [Arguments]     | Used for specifying `user keyword arguments`_.         |
      +-----------------+--------------------------------------------------------+
      | [Return]        | Used for specifying `user keyword return values`_.     |
      +-----------------+--------------------------------------------------------+
      | [Teardown]      | Used for specifying `user keyword teardown`_.          |
      +-----------------+--------------------------------------------------------+
      | [Timeout]       | Used for specifying a `user keyword timeout`_.         |
      +-----------------+--------------------------------------------------------+



.. table:: Keyword table에서 가능한 설정값
   :class: tabular

   +-----------------+--------------------------------------------------------+
   |      Name       |                         Description                    |
   +=================+========================================================+
   | [Documentation] | `user keyword documentation`_ 을 지정한다.             |
   +-----------------+--------------------------------------------------------+
   | [Tags]          | `user keyword tags`_ 를 지정한다.                      |
   +-----------------+--------------------------------------------------------+
   | [Arguments]     | `user keyword arguments`_ 를 지정한다.                 |
   +-----------------+--------------------------------------------------------+
   | [Return]        | `user keyword return values`_ 를 지정한다.             |
   +-----------------+--------------------------------------------------------+
   | [Teardown]      | `user keyword teardown`_ 를 지정한다.                  |
   +-----------------+--------------------------------------------------------+
   | [Timeout]       | `user keyword timeout`_ 를 지정한다.                   |
   +-----------------+--------------------------------------------------------+
