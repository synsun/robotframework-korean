.. _testdoc:

Test data documentation tool (Testdoc)
======================================

.. contents::
   :depth: 1
   :local:

..
   Testdoc is Robot Framework's built-in tool for generating high level
   documentation based on test cases. The created documentation is in HTML
   format and it includes name, documentation and other metadata of each
   test suite and test case, as well as the top-level keywords and their
   arguments.

Testdoc은 테스트 케이스에 기반한 상위 레벨 문서를 제작하는 로봇
프레임워크의 빌트인 도구이다. 생성된 문서는 HTML포맷이고, 이것은 이름,
설명, 각 테스트 스위트와 테스트 케이스의 다른 메타데이터, 전달인자의
상위레벨 키워드를 포함한다.

General usage
-------------

Synopsis
~~~~~~~~

::

    python -m robot.testdoc [options] data_sources output_file

Options
~~~~~~~

 -T, --title <title>           Set the title of the generated documentation.
                               Underscores in the title are converted to spaces.
                               The default title is the name of the top level suite.
 -N, --name <name>             Override the name of the top level test suite.
 -D, --doc <doc>               Override the documentation of the top level test suite.
 -M, --metadata <name:value>   Set/override free metadata of the top level test suite.
 -G, --settag <tag>            Set given tag(s) to all test cases.
 -t, --test <name>             Include tests by name.
 -s, --suite <name>            Include suites by name.
 -i, --include <tag>           Include tests by tags.
 -e, --exclude <tag>           Exclude tests by tags.
 -h, --help                    Print this help in the console.

..
   All options except :option:`--title` have exactly the same semantics as same
   options have when `executing test cases`__.


:option:`--title` 를 제외한 모든 옵션은 같은 옵션이 `executing test cases`__ 할 때와 정확히 같은 의미를 가진다.

__ `Configuring execution`_

Generating documentation
------------------------

..
   Data can be given as a single file, directory, or as multiple files and
   directories. In all these cases, the last argument must be the file where
   to write the output.

데이터는 단일 파일, 디렉토리, 다수의 파일, 디렉토리에 의해 주어진다.
모든 케이스에서, 마지막 전달인자는 아웃풋을 작성하는 파일이다.

..
   Testdoc works with all interpreters supported by Robot Framework (Python,
   Jython and IronPython). It can be executed as an installed module like
   `python -m robot.testdoc` or as a script like `python path/robot/testdoc.py`.

Testdoc은 로봇 프레임워크가 지원하는(Python,Jython,IronPhyton)
인터프리터와 작동한다. `python -m robot.testdoc` 와 같이 설치된 모듈
이나 `python path/robot/testdoc.py` 같은 스크립트 처럼 수행된다.

Examples::

  python -m robot.testdoc my_test.html testdoc.html
  jython -m robot.testdoc --name smoke_tests --include smoke path/to/my_tests smoke.html
  ipy path/to/robot/testdoc.py first_suite.txt second_suite.txt output.html
