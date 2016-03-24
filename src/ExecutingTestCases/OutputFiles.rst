Created outputs
===============

..
   Several output files are created when tests are executed, and all of
   them are somehow related to test results. This section discusses what
   outputs are created, how to configure where they are created, and how
   to fine-tune their contents.

테스트가 실행될 때 여러 출력 파일이 생성되며 이 모든 것은 테스트
결과와 다소 관련이 있다. 이 섹션은 어떤 출력이 생성되는지, 생성되는
장소를 어떻게 구성하는지 그리고 출력물의 내용은 어떻게 미세 조정하지를
설명한다.

.. contents::
   :depth: 2
   :local:

Different output files
----------------------

..
   This section explains what different output files can be created and
   how to configure where they are created. Output files are configured
   using command line options, which get the path to the output file in
   question as an argument. A special value `NONE`
   (case-insensitive) can be used to disable creating a certain output
   file.

이 섹션에서는 어떤 다른 출력 파일이 생성할 수 있는지와 어떻게 생성되는
곳을 설정할 수 있는지 설명한다. 출력 파일은 명령행 옵션을 사용하여
구성할 수 있어 전달인자로 출력문의 경로를 얻을 수 있다. 특수 문자
`NONE` (대소문자 구분없음)은 특정 출력파일을 생성하지 않도록 사용할 수
있다.



Output directory
~~~~~~~~~~~~~~~~

..
   All output files can be set using an absolute path, in which case they
   are created to the specified place, but in other cases, the path is
   considered relative to the output directory. The default output
   directory is the directory where the execution is started from, but it
   can be altered with the :option:`--outputdir (-d)` option. The path
   set with this option is, again, relative to the execution directory,
   but can naturally be given also as an absolute path. Regardless of how
   a path to an individual output file is obtained, its parent directory
   is created automatically, if it does not exist already.

모든 출력 파일은 지정된 장소에 생성하기 위해서 절대 경로를 사용하여
설정할 수 있다. 하지만 다른 경우 경로는 출력 디렉토리에 대한 상대
경로로 고려된다. 기본 출력 디렉토리는 실행이 시작되는 디렉토리 이지만
:option:`--outputdir (-d)` 로 변경 할 수 있다. 이 옵션으로 설정한
경로는 기본적으로 실행 디렉토리에 상대적이지만 절대 경로를 사용 할
수도 있다. 개별 출력 파일에 대한 경로를 얻는 방법에 관계 없이 이미
부모 디렉토리가 존재하지 않는다면 자동적으로 생성된다.

Output file
~~~~~~~~~~~

..
   Output files contain all the test execution results in machine readable XML
   format. Log_, report_ and xUnit_ files are typically generated based on them,
   and they can also be combined and otherwise post-processed with Rebot_.

출력 파일은 기계가 판독 가능한 XML 형식으로 모든 테스트 수행 결과를
포함한다. 일반적으로 Log_, report_ 와 xUnit_ 파일은 XML파일(output
files) 기초하여 생성되고, 그렇지 않으면 Rebot_ 으로 후처리하여 결합될
수 있다.

..
   .. tip:: Starting from Robot Framework 2.8, generating report_ and xUnit_
	    files as part of test execution does not anymore require processing
	    output files. Disabling log_ generation when running tests can thus
	    save memory.

.. tip:: Robot Framework 2.8부터 테스트 수행의 일부로 report_ 와
         xUnit_ 파일을 생성하는 것이 더 이상 출력 파일 처리를 요구하지
         않는다. 또한 테스트 수행시 log_ 생성을 비활성화 하여 메모리를
         절약할 수 있다.
	 
..
   The command line option :option:`--output (-o)` determines the path where
   the output file is created relative to the `output directory`_. The default
   name for the output file, when tests are run, is :file:`output.xml`.

명령행 :option:`--output (-o)` 옵션은 경로를 결정한다. 여기서 출력
파일은 `출력 디렉토리 <output directory_>`__ 에 상대적으로 생성된다.
테스트가 수행될 때 출력 파일의 기본 이름은 :file:`ouput.xml` 이다.

..
   When `post-processing outputs`_ with Rebot, new output files are not created
   unless the :option:`--output` option is explicitly used.

Rebot 으로 `출력 후처리 <post-processing outputs_>`__ 할 때 새로운
출력 파일은 명시적으로 :option:`--output` 옵션을 사용하지 않는한
생성되지 않는다.

..
   It is possible to disable creation of the output file when running tests by
   giving a special value `NONE` to the :option:`--output` option. Prior to Robot
   Framework 2.8 this also automatically disabled creating log and report files,
   but nowadays that is not done anymore. If no outputs are needed, they should
   all be explicitly disabled using `--output NONE --report NONE --log NONE`.

:option:`--output` 옵션에 `NONE` 값을 사용하여 테스트를 실행하면 출력
파일이 생성되지 않는다. Robot Framework 2.8 이전버전에서는 자동적으로
log 와 report 파일 생성하지 않았지만 지금은 더이상 이렇게 하지 않는다.
출력들이 필요 없다면 명시적으로 `--output NONE --report NONE --log
NONE` 을 사용하여 비활성화 해야 한다.

Log file
~~~~~~~~

..
   Log files contain details about the executed test cases in HTML
   format. They have a hierarchical structure showing test suite, test
   case and keyword details. Log files are needed nearly every time when
   test results are to be investigated in detail. Even though log files
   also have statistics, reports are better for
   getting an higher-level overview.

로그 파일은 HTML 형식으로 실행 된 test cases에 대한 자세한 정보를
포함하고, test suite, test case 와 keyword에 대한 상세정보를
계층적으로 보여준다. 로그 파일은 테스트 결과가 상세히 조사될 필요가
있는 거의 모든 경우에 필요하다. 또한 로그 파일은 통계를 가지고 있지만
더 높은 수준의 개요를 얻기 위해서는 리포트를 보는 것이 더 낫다.

..
   The command line option :option:`--log (-l)` determines where log
   files are created. Unless the special value `NONE` is used,
   log files are always created and their default name is
   :file:`log.html`.

명령행 :option:`--log (-l)` 옵션은 로그 파일이 생성될 곳을 결정한다.
`NONE` 값이 사용되지 않는 한 로그 파일은 항상 생성되고 기본 이름은
:file:`log.html` 이다.

.. figure:: src/ExecutingTestCases/log_passed.png
   :target: src/ExecutingTestCases/log_passed.html
   :width: 500

   로그 파일 시작 예제	   
..
      An example of beginning of a log file


      
.. figure:: src/ExecutingTestCases/log_failed.png
   :target: src/ExecutingTestCases/log_failed.html
   :width: 500

   키워드의 상세 정보를 보여주는 로그 파일 예제	   
..
      An example of a log file with keyword details visible

Report file
~~~~~~~~~~~

..
   Report files contain an overview of the test execution results in HTML
   format. They have statistics based on tags and executed test suites,
   as well as a list of all executed test cases. When both reports and
   logs are generated, the report has links to the log file for easy
   navigation to more detailed information.  It is easy to see the
   overall test execution status from report, because its background
   color is green, if all `critical tests`_ pass, and bright red
   otherwise.

리포트 파일은 HTML 형식으로 테스트 수행 결과의 개요을 포함하고, 태그를
기준으로 통계를 가지며 실행된 test suites 뿐만 아니라 실행된 test
cases의 모든 목록을 가진다. 리포트 및 로그를 생성할 때 리포트는 더욱
상세한 정보를 쉽게 탐색하기 위해 로그 파일에 대한 링크를 가진다. 모든
`critical tests`_ 가 PASS이면 배경 색은 녹색이고 그렇지 않다면 밝은
빨강이기 때문에 리포트로부터 전체 테스트 실행 상태를 보기는 것은 쉽다.

..
   The command line option :option:`--report (-r)` determines where
   report files are created. Similarly as log files, reports are always
   created unless `NONE` is used as a value, and their default
   name is :file:`report.html`.

명령행 :option:`--reprot (-r)` 옵션은 리포트 파일이 생성될 곳을
결정한다. 로그 파일과 비슷하게 리포트는 `NONE` 값을 사용하지 않는 한
항상 생성되고 그 기본 이름은 :file:`report.html` 이다.

.. figure:: src/ExecutingTestCases/report_passed.png
   :target: src/ExecutingTestCases/report_passed.html
   :width: 500

   성공적인 테스트 실행의 리포트 파일 예제
..
     An example report file of successful test execution

.. figure:: src/ExecutingTestCases/report_failed.png
   :target: src/ExecutingTestCases/report_failed.html
   :width: 500

   실패한 테스트 실행의 리포트 파일 예제
..
      An example report file of failed test execution

.. _xunit:

XUnit compatible result file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   XUnit result files contain the test execution summary in xUnit__ compatible
   XML format. These files can thus be used as an input for external tools that
   understand xUnit reports. For example, Jenkins__ continuous integration server
   supports generating statistics based on xUnit compatible
   results.

XUnit 결과 파일은 xUnit__ 호환 XML 형식으로 테스트 실행 요약을
포함한다. 그래서 이 파일은 xUnit 리포트를 이해하는 외부 툴의 입력으로
사용할 수 있다. 예를 들어 Jenkins__ 지속적인 통합 서버는 xUnit 호환
결과에 기초하여 통계를 생성하는 것을 지원한다.

..
   .. tip:: Jenkins also has a separate `Robot Framework plugin`__.

.. tip:: Jenkins는 또한 별도의 `Robot Framework plugin`__ 을 가진다.
	 
..
   XUnit output files are not created unless the command line option
   :option:`--xunit (-x)` is used explicitly. This option requires a path to
   the generated xUnit file, relatively to the `output directory`_, as a value.

XUnit 출력 파일은 명령행 :option:`--xunit (-x)` 옵션을 명시적으로
사용하지 않는 한 생성되지 않는다. 이 옵션은 `출력 디렉토리 <output
directory_>`__ 에 상대적인 값으로 생성된 xUnit 파일에 대한 경로를
요구한다.

..
   Because xUnit reports do not have the concept of `non-critical tests`__,
   all tests in an xUnit report will be marked either passed or failed, with no
   distinction between critical and non-critical tests. If this is a problem,
   :option:`--xunitskipnoncritical` option can be used to mark non-critical tests
   as skipped. Skipped tests will get a message containing the actual status and
   possible message of the test case in a format like `FAIL: Error message`.

xUnit 리포트는 `non-critical tests`__ 의 개념을 가지고 있지 않기
때문에 xUnit 리포트의 모든 테스트는 critical과 non-critical 테스트
사이의 차이없이 통과와 실패 중 하나로 표시됩니다. 이것이 문제가 된다면
:option:`--xunitskipnoncritical` 옵션으로 건너뛴(skipped) non-critical
테스트를 표시할 수 있다. Skipped tests는 실제 상태와 `FAIL: Error
message` 같은 형식의 test case 메시지를 얻게 된다.

..
   .. note:: :option:`--xunitskipnoncritical` is a new option in Robot Framework 2.8.
	  
.. note:: :option:`--xunitskipnoncritical` 은 Robot Framework 2.8에 도입되었다.
	     
__ http://en.wikipedia.org/wiki/XUnit
__ http://jenkins-ci.org
__ https://wiki.jenkins-ci.org/display/JENKINS/Robot+Framework+Plugin
__ `Setting criticality`_

Debug file
~~~~~~~~~~

..
   Debug files are plain text files that are written during the test
   execution. All messages got from test libraries are written to them,
   as well as information about started and ended test suites, test cases
   and keywords. Debug files can be used for monitoring the test
   execution. This can be done using, for example, a separate
   `fileviewer.py <https://bitbucket.org/robotframework/robottools/src/master/fileviewer/>`__
   tool, or in UNIX-like systems, simply with the ``tail -f`` command.

디버그 파일은 테스트 실행 중에 기록 된 일반 텍스트 파일이다. 테스트
라이브러리로부터 얻어진 모든 메시지가 기록될 뿐만 아니라 test suites,
test cases, keywords의 시작과 종료에 대한 정보를 포함한다. 디버그
파일은 테스트 실행을 모니터링하기 위해 사용 할 수 있다. 예를 들어
이것은 별도의 `fileviewer.py
<https://bitbucket.org/robotframework/robottools/src/master/fileviewer/>`__
툴이나 유닉스 계열의 시스템에서는 간단히 ``tail -f`` 명령어를 사용하여
모니터링 할 수 있다.

..
   Debug files are not created unless the command line option
   :option:`--debugfile (-b)` is used explicitly.

디버그 파일은 명령행 :option:`--debugfile (-b)` 옵션을 명시적으로
사용하지 않는한 생성되지 않는다.

Timestamping output files
~~~~~~~~~~~~~~~~~~~~~~~~~

..
   All output files listed in this section can be automatically timestamped
   with the option :option:`--timestampoutputs (-T)`. When this option is used,
   a timestamp in the format `YYYYMMDD-hhmmss` is placed between
   the extension and the base name of each file. The example below would,
   for example, create such output files as
   :file:`output-20080604-163225.xml` and :file:`mylog-20080604-163225.html`::

이 섹션에 나열된 모든 출력 파일은 :option:`--timestampoutputs (-T)`
옵션으로 자동적으로 시각 정보를 표시할 수 있다. 이 옵션을 사용할 경우
`YYYYMMDD-hhmmss` 형식의 타임스탬프가 확장자와 파일의 기본 이름 사이에
위치한다. 아래의 예는, 예를 들어 :file:`output-20080604-163225.xml` 와
:file:`mylog-20080604-163225.html` 같은 출력 파일을 생성한다::

   robot --timestampoutputs --log mylog.html --report NONE tests.html

Setting titles
~~~~~~~~~~~~~~

The default titles for logs_ and reports_ are generated by prefixing
the name of the top-level test suite with :name:`Test Log` or
:name:`Test Report`. Custom titles can be given from the command line
using the options :option:`--logtitle` and :option:`--reporttitle`,
respectively. Underscores in the given titles are converted to spaces
automatically.

logs_ 와 reports_ 의 기본 제목은 최상위의 test suite 의 이름을
접두어로 하고 :name:`Test Log` 나 :name:`Test Report` 를 뒤에 붙인다.
사용자 정의 제목은 명령행에서 :option:`--logtile` 과
:option:`--reporttile` 옵션을 사용하여 각각 바꿀 수 있다. 주어진
제목에 언더스코어가 있으면 자동적으로 공백으로 변경한다.

Example::

   robot --logtitle Smoke_Test_Log --reporttitle Smoke_Test_Report --include smoke my_tests/

Setting background colors
~~~~~~~~~~~~~~~~~~~~~~~~~

..
   By default the `report file`_ has a green background when all the
   `critical tests`_ pass and a red background otherwise.  These colors
   can be customized by using the :option:`--reportbackground` command line
   option, which takes two or three colors separated with a colon as an
   argument::

기본적으로 `report file`_ 은 모든 `critical tests` 가 PASS하면 녹색
배경을 가지고 그렇지 않은면 빨간색 배경을 가진다. 이런 색상은
:option:`--reportbackground` 명령행 옵션에 두개 또는 세개의 콜론으로
분리된 색상을 전달인자로 사용하여 설정할 수 있다::

   --reportbackground blue:red
   --reportbackground green:yellow:red
   --reportbackground #00E:#E00

..
   If you specify two colors, the first one will be used instead of the
   default green color and the second instead of the default red. This
   allows, for example, using blue instead of green to make backgrounds
   easier to separate for color blind people.

두가지 색상을 지정하면 첫번째는 기본 녹색 대신, 두번째는 기본 빨간색
대신 사용된다. 예를 들어 색맹인 사람이 색상을 분리하기 쉽게하기 위해
녹색 대신 파란색을 사용할 수도 있다.

..
   If you specify three colors, the first one will be used when all the
   test succeed, the second when only non-critical tests have failed, and
   the last when there are critical failures. This feature thus allows
   using a separate background color, for example yellow, when
   non-critical tests have failed.

세가지 색상을 지정하면 첫번째는 모든 테스트가 성공했을 때 사용하고
두번째는 단지 non-critical 테스트만 실패했을 때 사용하고 마지막은
critical 실패가 있을 때 사용한다. 그래서 이 기능은 분리된 배경 색깔
예를 들어 non-critical 테스트가 실패했을 때 노랑을 사용 할 수 있다.

..
   The specified colors are used as a value for the `body`
   element's `background` CSS property. The value is used as-is and
   can be a HTML color name (e.g. `red`), a hexadecimal value
   (e.g. `#f00` or `#ff0000`), or an RGB value
   (e.g. `rgb(255,0,0)`). The default green and red colors are
   specified using hexadecimal values `#9e9` and `#f66`,
   respectively.

지정된 색상은 `body` 엘리먼트의 `boackground` CSS 속성의 값으로 사용
할 수 있다. 이값은 HTML 색 이름(예, `red`), 16 진수 값(예, `#f00` 또는
`#ff0000`), 또는 RGB 값(예, `rgb(255,0,0)`)을 사용 할 수 있다. 기본
녹색과 빨간색은 각각 6 진수 값 `#9e9` 과 `#f66` 으로 지정할 수 있다.

Log levels
----------

Available log levels
~~~~~~~~~~~~~~~~~~~~

..
   Messages in `log files`_ can have different log levels. Some of the
   messages are written by Robot Framework itself, but also executed
   keywords can `log information`__ using different levels. The available
   log levels are:

`log files`_ 의 메시지는 다른 로그 레벨을 가진다. 메시지의 일부는
Robot Framework 자체에서 작성될 뿐만 아니라 실행된 키워드는 다른 로그
레벨을 사용하여 `정보를 기록`__ 할 수 있다. 사용가능한 로그 레벨은
다음과 같다:

..
   `FAIL`
      Used when a keyword fails. Can be used only by Robot Framework itself.

`FAIL`
   키워드가 실패할 때 사용. Robot Framework 자체만 사용 가능.

..
   `WARN`
      Used to display warnings. They shown also in `the console and in
      the Test Execution Errors section in log files`__, but they
      do not affect the test case status.

`WARN`
   경고를 표시하는 데 사용. `콘솔 및 로그 파일에서 테스트 실행 오류
   섹션`__ 에도 보여지나 test case 상태에 영향을 미치지 않는다.
   
..
   `INFO`
      The default level for normal messages. By default,
      messages below this level are not shown in the log file.

`INFO`
   정상적인 메시지의 기본 레벨. 기본적으로 이 레벨 아래의 메시지는
   로그 파일에 보여지지 않는다.

..
   `DEBUG`
      Used for debugging purposes. Useful, for example, for
      logging what libraries are doing internally. When a keyword fails,
      a traceback showing where in the code the failure occurred is
      logged using this level automatically.

`DEBUG`
   디버깅 목적으로 사용. 예를 들어 로깅을 위해 어떤 라이브러리가
   내부적으로 사용될 경우 유용하다. 키워드가 실패한 경우 실패가 발생한
   코드의 역추적이 이 레벨을 사용자여 자동적으로 기록된다.
   
..
   `TRACE`
      More detailed debugging level. The keyword arguments and return values
      are automatically logged using this level.

`TRACE`
   더 자세한 디버깅 레벨. 키워드 인수 및 반환 값이 자동적으로 이
   레벨을 사용할때 기록된다.

   
__ `Logging information`_
__ `Errors and warnings during execution`_

Setting log level
~~~~~~~~~~~~~~~~~

..
   By default, log messages below the `INFO` level are not logged, but this
   threshold can be changed from the command line using the
   :option:`--loglevel (-L)` option. This option takes any of the
   available log levels as an argument, and that level becomes the new
   threshold level. A special value `NONE` can also be used to
   disable logging altogether.

기본적으로 `INFO` 레벨 아래의 로그 메시지는 기록하지 않는다. 이
임계값은 명령행 :option:`--loglevel (-L)` 옵션을 사용하여 변경할 수
있다. 이 옵션은 가능한 로그 레벨을 인자로 받아서 새로운 임계치 레벨을
설정한다. `NONE` 값은 모든 로깅을 비활성화하는데 사용한다.

..
   It is possible to use the :option:`--loglevel` option also when
   `post-processing outputs`_ with ``rebot``. This allows, for example,
   running tests initially with the `TRACE` level, and generating smaller
   log files for normal viewing later with the `INFO` level. By default
   all the messages included during execution will be included also with
   ``rebot``. Messages ignored during the execution cannot be recovered.

``rebot`` 으로 `출력을 사후 처리 <post-processing outputs_>`__ 할때도
:option:`--loglevel` 옵션을 사용할 수 있다. 예를 들어 초기에 `TRACE`
레벨로 테스트를 실행하고, 후에 `INFO` 레벨로 일반적인 보기를 위한 더
작은 로그 파일을 생성할 수 있다. 기본적으로 실행동안에 포함된 모든
메시지는 ``rebot`` 을 사용해도 포함 된다. 실행동안 무시되는 메시지는
복구 할 수 없다.

..
   Another possibility to change the log level is using the BuiltIn_
   keyword :name:`Set Log Level` in the test data. It takes the same
   arguments as the :option:`--loglevel` option, and it also returns the
   old level so that it can be restored later, for example, in a `test
   teardown`_.

로그 레벨을 변경하는 다른 방법은 테스트 데이터에서 BuiltIn_ 키워드
:name:`Set Log Level` 을 사용하는 것이다. 이것은 :option:`--loglevel`
옵션과 동일한 인자를 취할 수 있고 나중에 예를 들어, `test teardown`_
에서 복원하기 위해 이전 레벨을 반환한다.

Visible log level
~~~~~~~~~~~~~~~~~

..
   Starting from Robot Framework 2.7.2, if the log file contains messages at
   `DEBUG` or `TRACE` levels, a visible log level drop down is shown
   in the upper right corner. This allows users to remove messages below chosen
   level from the view. This can be useful especially when running test at
   `TRACE` level.

Robot Framework 2.7.2 부터 로그 파일은 `DEBUG` 또는 `TRACE` 레벨
메시지를 포함하는 경우 오른 쪽 상단에 선택가능한 로그 레벨 드롭 다운을
보여준다. 사용자가 선택한 레벨 아래의 메시지는 제거된다. 이것은 특히
실행 테스트가 `TRACE` 레벨일 때 유용하다.

.. figure:: src/ExecutingTestCases/visible_log_level.png
   :target: src/ExecutingTestCases/visible_log_level.html
   :width: 500

   An example log showing the visible log level drop down

..
   By default the drop down will be set at the lowest level in the log file, so
   that all messages are shown. The default visible log level can be changed using
   :option:`--loglevel` option by giving the default after the normal log level
   separated by a colon::

로그 파일에서 드롭 다운은 기본적으로 모든 메시기가 보여지도록 가장
낮은 레벨로 설정한다. 기본적으로 보여주는 로그 레벨은
:option:`--loglevel` 옵션에 로그 레벨 후에 콜론으로 구분하여
기본레벨을 줌으로써 변경할 수 있다::

   --loglevel DEBUG:INFO

..
   In the above example, tests are run using level `DEBUG`, but
   the default visible level in the log file is `INFO`.

위의 예제에서 테스트는 `DEBUG` 레벨을 사용하여 수행하지만 로그파일에서
기본 표시 레벨은 `INFO` 이다.


Splitting logs
--------------

..
   Normally the log file is just a single HTML file. When the amount of he test
   cases increases, the size of the file can grow so large that opening it into
   a browser is inconvenient or even impossible. Hence, it is possible to use
   the :option:`--splitlog` option to split parts of the log into external files
   that are loaded transparently into the browser when needed.

일반적으로 로그 파일은 단일 HTML 파일이다. Test cases의 양이 증가함에
따라 파일의 크기는 브라우저로 열기 불편하거나 심지어 불가능할 정도로
커질 수 있다. 그러므로 필요한 경우 :option:`--splitlog` 옵션을
사용해서 로그 파일을 여러 외부 파일로 분할하여 필요할때 로드 할 수
있다.
   
..
   The main benefit of splitting logs is that individual log parts are so small
   that opening and browsing the log file is possible even if the amount
   of the test data is very large. A small drawback is that the overall size taken
   by the log file increases.

분할한 로그의 주요 이점은 테스트 데이타의 양이 매우 큰 경우에도 개별
로그 부분이 작아서 열어서 브라우징할 수 있다는 것이다. 단점은 로그
파일의 전체 크기가 증가한다는 것이다.
   
..
   Technically the test data related to each test case is saved into
   a JavaScript file in the same folder as the main log file. These files have
   names such as :file:`log-42.js` where :file:`log` is the base name of the
   main log file and :file:`42` is an incremented index.

기술적으로 각각의 test case에 관련된 테스트 테이타는 주 로그 파일과
같은 폴더에 자바스크립트 파일로 저장된다. 파일은 :file:`log-42.js` 와
같은 이름을 가진다. 여기서 :file:`log` 는 주 로그 파일의 기본 이름
이고, :file:`42` 는 증분 인덱스 이다.
   
..
   .. note:: When copying the log files, you need to copy also all the
	     :file:`log-*.js` files or some information will be missing.

.. note:: 로그 파일을 복사할 때 모든 :file:`log-*.js` 파일들을
          복사해야 한다. 그렇지 않으면 몇몇 정보는 누락된다.

Configuring statistics
----------------------

..
   There are several command line options that can be used to configure
   and adjust the contents of the :name:`Statistics by Tag`, :name:`Statistics
   by Suite` and :name:`Test Details by Tag` tables in different output
   files. All these options work both when executing test cases and when
   post-processing outputs.

:name:`Statistics by Tag`, :name:`Statistics by Suite`, :name:`Test
Details by Tag` 표의 내용을 구성하고 조정하기 위해 사용할 수 있는
여러가기 명령행 옵션이 있다. 이러한 모든 옵션은 test cases를 수행할
때와 출력을 후처리 할때 모두 동일하게 작동한다.


Configuring displayed suite statistics
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   When a deeper suite structure is executed, showing all the test suite
   levels in the :name:`Statistics by Suite` table may make the table
   somewhat difficult to read. By default all suites are shown, but you can
   control this with the command line option :option:`--suitestatlevel` which
   takes the level of suites to show as an argument::

여러 단계의 suite 구조가 실행되면 :name:`Statistics by Suite` 표는
모든 test suite 레벨을 보여주기 때문에 표를 읽는 것을 다소 어렵게
만든다. 기본적으로 모든 suite이 보여지지만 명령행에서
:option:`--suitestatlevel` 옵션으로 제어 할 수 있다. 이 옵션은
전달인자로 보기 위한 suites의 레벨을 받는다::
   
    --suitestatlevel 3

Including and excluding tag statistics
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   When many tags are used, the :name:`Statistics by Tag` table can become
   quite congested. If this happens, the command line options
   :option:`--tagstatinclude` and :option:`--tagstatexclude` can be
   used to select which tags to display, similarly as
   :option:`--include` and :option:`--exclude` are used to `select test
   cases`__::

많은 태그를 사용하는 경우 :name:`Statistics by Tag` 표는 매우
혼잡하다. 이 경우 명령행 옵션 :option:`--tagstatinclude` 와
:option:`--tagstatexclude` 을 표시할 태그를 선택하는 데 사용할 수
있다. 이것은 `test cases 선택`__ 하는데 사용하는 :option:`--include`
와 :option:`--exclude` 옵션과 사용하는 것이 비슷하다::

   --tagstatinclude some-tag --tagstatinclude another-tag
   --tagstatexclude owner-*
   --tagstatinclude prefix-* --tagstatexclude prefix-13

__ `By tag names`_

Generating combined tag statistics
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   The command line option :option:`--tagstatcombine` can be used to
   generate aggregate tags that combine statistics from multiple
   tags. The combined tags are specified using `tag patterns`_ where
   `*` and `?` are supported as wildcards and `AND`,
   `OR` and `NOT` operators can be used for combining
   individual tags or patterns together.

명령행 옵션 :option:`--tagstatcombine` 은 다중 태그로 부터 통계를
결합하여 태그를 생성할 수 있다. 결합 태그는 `tag patterns`_ 을
사용하여 지정된다. 이 경우 와일드카드로 `*` 와 `?` 를 사용할 수 있고,
개별 태그와 패턴을 결합하기 위해서 `AND`, `OR`, `NOT` 연산자를 함께
사용할 수 있다.
   
..
   The following examples illustrate creating combined tag statistics using
   different patterns, and the figure below shows a snippet of the resulting
   :name:`Statistics by Tag` table::

다음 예는 다른 패턴을 사용하여 결합 태그 통계 작성을 예시한다. 아래의
그림은 :name:`Statistics by Tag` 표 결과의 단편을 보여준다::
	   
    --tagstatcombine owner-*
    --tagstatcombine smokeANDmytag
    --tagstatcombine smokeNOTowner-janne*

.. figure:: src/ExecutingTestCases/tagstatcombine.png
   :width: 550

   Examples of combined tag statistics

..
   As the above example illustrates, the name of the added combined statistic
   is, by default, just the given pattern. If this is not good enough, it
   is possible to give a custom name after the pattern by separating them
   with a colon (`:`). Possible underscores in the name are converted
   to spaces::

위의 예에서 보듯이 추가로 결합된 통계의 이름은 기본적으로 주어진
패턴이다. 이것으로 충분하지 않으면 패턴 뒤에 콜론(`:`)을 구분자로
사용자 정의 이름을 제공할 수 있다. 이름에서 언더스코어는 공백으로
치환한다::
     
    --tagstatcombine prio1ORprio2:High_priority_tests

Creating links from tag names
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   You can add external links to the :name:`Statistics by Tag` table by
   using the command line option :option:`--tagstatlink`. Arguments to this
   option are given in the format `tag:link:name`, where `tag`
   specifies the tags to assign the link to, `link` is the link to
   be created, and `name` is the name to give to the link.

명령행 옵션 :option:`--tagstatlink` 로 :name:`Statistics by Tag` 표에
외부링크를 추가할 수 있다. 이 옵션에 대한 전달인자는 `tag:link:name`
형식으로 주어진다. 여기서 `tag` 는 링크에 할당하는 태그를, `link` 는
생성될 링크를, `name` 는 링크에 주어진 이름을 지정한다.
   
..
   `tag` may be a single tag, but more commonly a `simple pattern`_
   where `*` matches anything and `?` matches any single
   character. When `tag` is a pattern, the matches to wildcards may
   be used in `link` and `title` with the syntax `%N`,
   where "N" is the index of the match starting from 1.

`tag` 는 단일 태그이지만 더 일반적으로 `간단한 패턴 <simple
pattern_>`__ 이 될 수 있다. 여기서 `*` 는 어떤 것이든 일치하는 것을,
`?` 는 단지 하나의 문자와 일치하는 것을 뜻한다. `tag` 가 패턴일 경우
와일드카드에 매치하고, `%N` 문법은 `link` 와 `title` 에 사용된다.
여기서 "N"은 1부터 시작한 매치 인덱스이다.
   
..
   The following examples illustrate the usage of this option, and the
   figure below shows a snippet of the resulting :name:`Statistics by
   Tag` table when example test data is executed with these options::

다음 예는 이 옵션의 사용법을 보여준다. 아래 그림은 테스트 데이타
예제가 이 옵션으로 실행되었을 때 :name:`Statistics by Tag` 표 결과중
일부를 보여 준다::
     
    --tagstatlink mytag:http://www.google.com:Google
    --tagstatlink jython-bug-*:http://bugs.jython.org/issue_%1:Jython-bugs
    --tagstatlink owner-*:mailto:%1@domain.com?subject=Acceptance_Tests:Send_Mail

.. figure:: src/ExecutingTestCases/tagstatlink.png
   :width: 550

   Examples of links from tag names

Adding documentation to tags
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Tags can be given a documentation with the command line option
   :option:`--tagdoc`, which takes an argument in the format
   `tag:doc`. `tag` is the name of the tag to assign the
   documentation to, and it can also be a `simple pattern`_ matching
   multiple tags. `doc` is the assigned documentation. Underscores
   in the documentation are automatically converted to spaces and it
   can also contain `HTML formatting`_.

태그는 `tag:doc` 형식으로 전달인자를 취하는 명령행 옵션
:option:`--tagdoc` 으로 문서에 주어질 수 있다. `tag` 문서에 할당하는
태그의 이름이며 다중 태그 일치를 위해 `간단한 패턴 <simple
pattern_>`__ 을 사용할 수 있다. `doc` 은 문서에 할당된다. 문서에서
언더스코어는 자동적으로 공백으로 변화되고 `HTML 형식 <HTML
formatting_>`__ 을 포함할 수 있다.
   
..
   The given documentation is shown with matching tags in the :name:`Test
   Details by Tag` table, and as a tool tip for these tags in the
   :name:`Statistics by Tag` table. If one tag gets multiple documentations,
   they are combined together and separated with an ampersand.

주어진 문서는 :name:`Test Details by Tag` 표에서 일치하는 태그와 함께
보여진다. :name:`Statistics by Tag` 표에서 이런 태그의 툴팁으로
보여준다. 하나의 태그가 여러 문서를 가진다면 함께 결합하거나
앰퍼센트로 구분된다.

Examples::

    --tagdoc mytag:My_documentation
    --tagdoc regression:*See*_http://info.html
    --tagdoc owner-*:Original_author

Removing and flattening keywords
--------------------------------

..
   Most of the content of `output files`_ comes from keywords and their
   log messages. When creating higher level reports, log files are not necessarily
   needed at all, and in that case keywords and their messages just take space
   unnecessarily. Log files themselves can also grow overly large, especially if
   they contain `for loops`_ or other constructs that repeat certain keywords
   multiple times.

`output files`_ 의 대부분의 내용은 키워드와 해당 로그 메시지에서 온다.
높은 수준의 레포트와 로그 파일을 생성할 때 모든 것이 필요하지는
않으며, 그 경우 키워드와 해당 메시지는 단지 불필요한 공간을 차지한다.
로그 파일 자체가 지나치게 커질 수 있다. 특히 `for loops`_ 나 특정
키워드를 여러번 반복하는 구조를 포함 할 경우는 더욱 커진다.

..
   In these situations, command line options :option:`--removekeywords` and
   :option:`--flattenkeywords` can be used to dispose or flatten unnecessary keywords.
   They can be used both when `executing test cases`_ and when `post-processing
   outputs`_. When used during execution, they only affect the log file, not
   the XML output file. With `rebot` they affect both logs and possibly
   generated new output XML files.

이런 상황에서 명령행 옵션 :option:`--removekeywords` 와
:option:`--flattenkeywords` 가 불필요한 키워드를 버리거나
평평하게(flatten) 하는데 사용 할 수 있다. `test cases 실행 <executing
test cases_>`__ 할 때와 `출력 후처리 <post-processing outputs_>`__ 할
때 모두 사용 할 수 있다. 테스트 실행 중에 사용하는 경우 단지 로그
파일에만 영향을 미치고 XMM 출력 파일에는 영향을 미치지 않는다. `rebot`
을 해당 옵션과 같이 사용하면 로그와 생성된 새로운 출력 XML파일 모두에
영향을 준다.
   
Removing keywords
~~~~~~~~~~~~~~~~~

..
   The :option:`--removekeywords` option removes keywords and their messages
   altogether. It has the following modes of operation, and it can be used
   multiple times to enable multiple modes. Keywords that contain `errors
   or warnings`__ are not removed except when using the `ALL` mode.

:option:`--removekeywords` 옵션은 키워드와 해당 메시지를 같이
제거한다. 이것은 다음의 여러 동작 모드를 가지고 다수의 모드를 활성화
하기 위해 여러번 사용 될 수 있다. `에러 또는 경고`__ 를 포함하는
키워드는 `ALL` 모드를 사용하지 않는 한 제거되지 않는다.

`ALL`
   무조건 모든 키워드에서 데이터를 제거.

..
      Remove data from all keywords unconditionally.

`PASSED`
   성공한 test cases에서 키워드 데이타 제거. 대부분의 경우에 이 옵션을
   사용하여 생성된 로그 파일은 실패를 조사하기 위한 충분한 정보를
   포함한다.
   
..
      Remove keyword data from passed test cases. In most cases, log files
      created using this option contain enough information to investigate
      possible failures.

`FOR`
   마지막 하나를 제외한 `for loops`_ 에서 모든 성공한 반복을 제거.
   
..
      Remove all passed iterations from `for loops`_ except the last one.

`WUKS`
   마지막 하나를 제외한 BuiltIn_ 키워드 :name:`Wait Until Keyword
   Succeeds` 내의 모든 실패한 키워드 제거.

..
      Remove all failing keywords inside BuiltIn_ keyword
      :name:`Wait Until Keyword Succeeds` except the last one.

`NAME:<pattern>`
   키워드 상태에 관계 없이 주어진 패턴에 일치하는 모든 키워드의 데이타
   제거. 패턴은 키워드의 라이브러리나 리소스 파일 이름을 접두어로
   가지는 완전한 이름에 일치된다. 패턴은 대소문자, 공백, 언더스코어를
   구분하지 않고, 와일드 카드로 `*` 와 `?` 를 사용하는 `간단한 패턴
   <simple pattern_>`__ 을 지원한다.

..
      Remove data from all keywords matching the given pattern regardless the
      keyword status. The pattern is
      matched against the full name of the keyword, prefixed with
      the possible library or resource file name. The pattern is case, space, and
      underscore insensitive, and it supports `simple patterns`_ with `*`
      and `?` as wildcards.

`TAG:<pattern>`

   주어진 패턴에 일치하는 태그를 가지는 키워드의 데이타를 제거. 태그는
   대소문자, 공백에 무관하고 태그 패턴을 사용하여 지정할 수 있다.
   와일드카드로 `*` 와 `?` 을 지원하고, `AND`, `OR`, `NOT` 연산자를
   사용하여 개별 태그나 패턴을 같이 결합하여 사용할 수 있다.
   `라이브러리 키워드 태그`__ 와 `사용자 키워드 태그 <user keyword
   tags_>`__ 모두 사용할 수 있다.

..
      Remove data from keywords with tags that match the given pattern. Tags are
      case and space insensitive and they can be specified using `tag patterns`_
      where `*` and `?` are supported as wildcards and `AND`, `OR` and `NOT`
      operators can be used for combining individual tags or patterns together.
      Can be used both with `library keyword tags`__ and `user keyword tags`_.

Examples::

   rebot --removekeywords all --output removed.xml output.xml
   robot --removekeywords passed --removekeywords for tests.txt
   robot --removekeywords name:HugeKeyword --removekeywords name:resource.* tests.txt
   robot --removekeywords tag:huge tests.txt

..
   Removing keywords is done after parsing the `output file`_ and generating
   an internal model based on it. Thus it does not reduce memory usage as much
   as `flattening keywords`_.

키워드 제거는 `output file`_ 파싱과 그것에 기초하여 내부 모델을 생성한
후에 완료된다. 따라서 `flattening keywords`_ 만큼 메모리 사용량을
감소하지 않는다.
   
__ `Errors and warnings`_
__ `Keyword tags`_

..
   .. note:: The support for using :option:`--removekeywords` when executing tests
	     as well as `FOR` and `WUKS` modes were added in Robot
	     Framework 2.7.

.. note:: 테스트 수행시 :option:`--removekeywords` 옵션이 `FOR` 와
          `WUKS` 를 지원한 것은 Robot Framework 2.7 부터이다.
	     
..
   .. note:: `NAME:<pattern>` mode was added in Robot Framework 2.8.2 and
	     `TAG:<pattern>` in 2.9.

.. note:: `NAME:<patten>` 모드는 Robot Framework 2.8.2,
          `TAG:<pattern>` 은 2.9 에 추가되었다.

   
Flattening keywords
~~~~~~~~~~~~~~~~~~~

..
   The :option:`--flattenkeywords` option flattens matching keywords. In practice
   this means that matching keywords get all log messages from their child
   keywords, recursively, and child keywords are discarded otherwise. Flattening
   supports the following modes:

:option:`--flattenkeywords` 옵션은 일치하는 키워드를 평탄화한다.
실제로 이는 일치하는 키워드는 반복적으로 그들의 자식 키워드로부터 모든
로그 메시지를 얻고 그렇지 않으면 자식 키워드는 버려지는 것을 의미한다.
평탄화는 다음 모드를 지원한다:

..
   `FOR`
      Flatten `for loops`_ fully.

   `FORITEM`
      Flatten individual for loop iterations.

   `NAME:<pattern>`
      Flatten keywords matching the given pattern. Pattern matching rules are
      same as when `removing keywords`_ using `NAME:<pattern>` mode.

   `TAG:<pattern>`
      Flatten keywords with tags matching the given pattern. Pattern matching
      rules are same as when `removing keywords`_ using `TAG:<pattern>` mode.

`FOR`
    `for loops` 를 완전히 평탄화.

`FORITEM`
    개별 for loop 반복 평탄화.

`NAME:<pattern>`
   주어진 패턴에 일치하는 키워드 평탄화. 패턴 매칭 규칙은
   `NAME:<pattern>` 모드를 사용하여 `removing keywords`_ 할 때와
   동일

`TAG:<pattern>`
   주어진 패턴에 일치하는 태그를 가지는 키워드 평탄화. 패턴 매칭
   규칙은 `TAG:<pattern>` 모드를 사용하여 `removing keywords`_ 할 때와
   동일
   
Examples::

   robot --flattenkeywords name:HugeKeyword --flattenkeywords name:resource.* tests.txt
   rebot --flattenkeywords foritem --output flattened.xml original.xml

..
   Flattening keywords is done already when the `output file`_ is parsed
   initially. This can save a significant amount of memory especially with
   deeply nested keyword structures.

`output file`_ 이 처음 파싱될 때 키워드를 평탄화는 이미 이루어진다.
이는 특히 깊게 중첩된 키워드 구조를 가질때 상당한 양의 메모리를 절약
할 수 있다.

..
   .. note:: Flattening keywords is a new feature in Robot Framework 2.8.2, `FOR`
	     and `FORITEM` modes were added in 2.8.5 and `TAG:<pattern>` in 2.9.

.. note:: 키워드 평탄화는 Robot Framework 2.8.2에 도입되었다. `FOR` 와
          `FORITEM` 모드는 2.8.5에 추가되고 `TAG:<pattern>` 은 2.9에
          도입되었다.
	     
Setting start and end time of execution
---------------------------------------

..
   When `combining outputs`_ using ``rebot``, it is possible to set the start
   and end time of the combined test suite using the options :option:`--starttime`
   and :option:`--endtime`, respectively. This is convenient, because by default,
   combined suites do not have these values. When both the start and end time are
   given, the elapsed time is also calculated based on them. Otherwise the elapsed
   time is got by adding the elapsed times of the child test suites together.

``rebot`` 을 사용하여 `출력을 결합 <combining outputs_>`__ 할 때
:option:`--starttime` 과 :option:`--endtime` 옵션을 각각 사용하여
결합된 test suite 의 시작과 종료 시간을 설정 할 수 있다. 기본적으로
결합된 suites는 이러한 값이 없기 때문에 편리하다. 시작과 종료 시간
모두 주어질 경우 경과 시간은 그들에 기초하여 계산된다. 그렇지 않으면
경과 시간은 자식 test suites의 경과 시간을 함께 추가함으로써 얻어질 수
있다.

..
   It is also possible to use the above mentioned options to set start and end
   times for a single suite when using ``rebot``.  Using these options with a
   single output always affects the elapsed time of the suite.

위에 언급한 옵션은 ``rebot`` 을 사용할 때 하나의 suite을 위한 시작과
종료 시간을 설정하는데 사용할 수 있다. 하나의 출력에 이 옵션을
사용하는 것은 항상 suite의 경과 시간에 영향을 미친다.
   
..
   Times must be given as timestamps in the format `YYYY-MM-DD
   hh:mm:ss.mil`, where all separators are optional and the parts from
   milliseconds to hours can be omitted. For example, `2008-06-11
   17:59:20.495` is equivalent both to `20080611-175920.495` and
   `20080611175920495`, and also mere `20080611` would work.

시간은 `YYYY-MM-DD hh:mm:ss.mil` 형식의 타임스탬프로 주어져야 한다.
여기서 모든 분리자는 선택적이고 밀리세컨드에서 시간까지의 부분은
생략될 수 있다. 예를 들어 `2008-06-11 17:59:20.495` 는
`20080611-175920.495` 또는 `20080611175920495` 둘 다와 동일하다. 또한
단순한 `20080611` 또한 동작한다.
   
Examples::

   rebot --starttime 20080611-17:59:20.495 output1.xml output2.xml
   rebot --starttime 20080611-175920 --endtime 20080611-180242 *.xml
   rebot --starttime 20110302-1317 --endtime 20110302-11418 myoutput.xml

Programmatic modification of results
------------------------------------

..
   If the provided built-in features to modify results are are not enough,
   Robot Framework 2.9 and newer provide a possible to do custom modifications
   programmatically. This is accomplished by creating a model modifier and
   activating it using the :option:`--prerebotmodifier` option.

결과를 변경하기 위해 제공되는 내장 기능 충분하지 않으면 Robot
Framework 2.9 및 최신 버전은 프로그래밍으로 사용자 정의 변경을 할 수
있도록 제공한다. 이는 모델 변경자를 생성함으로써 이루어지고
:option:`--prerebotmodifier` 옵션을 사용하여 활성화 할 수 있다.

..
   This functionality works nearly exactly like `programmatic modification of
   test data`_ that can be enabled with the :option:`--prerunmodifier` option.
   The only difference is that the modified model is Robot Framework's
   result model and not the executable test suite model. For example, the
   following modifier marks all passed tests that have taken more time than
   allowed as failed:

이러한 기능은 :option:`--prerunmodifier` 옵션으로 활성화 할 수 있는
`테스트 테이타의 프로그램 변경 <programmatic modification of test
data_>`__ 처럼 거의 정확하게 작동한다. 유일한 차이점은 변경된 모델은
Robot Framework의 결과 모델이지 수행가능한 test suite 모델은
아니다라는 것이다. 예를 들어 다음의 변경자는 실패로 허용 된 것 보다 더
많은 시간을 취하는 모든 성공한 tests를 표시한다:

.. sourcecode:: python

    from robot.api import SuiteVisitor


    class ExecutionTimeChecker(SuiteVisitor):

        def __init__(self, max_seconds):
            self.max_milliseconds = float(max_seconds) * 1000

        def visit_test(self, test):
            if test.status == 'PASS' and test.elapsedtime > self.max_milliseconds:
                test.status = 'FAIL'
                test.message = 'Test execution took too long.'

..
   If the above modifier would be in file :file:`ExecutionTimeChecker.py`, it
   could be used, for example, like this::

위의 변경자가 :file:`ExecutionTimeChecker.py` 파일안에 존재 하면 예를
들어 다음과 같이 사용될 수 있다::

    # Specify modifier as a path when running tests. Maximum time is 42 seconds.
    robot --prerebotmodifier path/to/ExecutionTimeChecker.py:42 tests.robot

    # Specify modifier as a name when using Rebot. Maximum time is 3.14 seconds.
    # ExecutionTimeChecker.py must be in the module search path.
    rebot --prerebotmodifier ExecutionTimeChecker:3.14 output.xml

..
   If more than one model modifier is needed, they can be specified by using
   the :option:`--prerebotmodifier` option multiple times. When executing tests,
   it is possible to use :option:`--prerunmodifier` and
   :option:`--prerebotmodifier` options together.

한 모델 이상의 변경자가 필요하다면 :option:`--prerebotmodifier` 옵션을
여러번 사용하여 지정할 수 있다. 테스트를 수행 할 때
:option:`--prerebotmodifier` 와 :option:`--prerebotmodifier` 옵션을
함께 사용할 수 있다.

System log
----------

..
   Robot Framework has its own plain-text system log where it writes
   information about

   - Processed and skipped test data files
   - Imported test libraries, resource files and variable files
   - Executed test suites and test cases
   - Created outputs

Robot Framework은 다음과 같은 정보를 적는 자신의 일반 텍스트 시스템
로그를 가진다.

   - 처리 및 건너뛴 테스트 데이타 파일
   - 임포트된 test libraries, resource files, variable files
   - 실행된 test suites와 test cases
   - 생성된 출력  
     
..
   Normally users never need this information, but it can be
   useful when investigating problems with test libraries or Robot Framework
   itself. A system log is not created by default, but it can be enabled
   by setting the environment variable ``ROBOT_SYSLOG_FILE`` so
   that it contains a path to the selected file.

일반적으로 사용자는 이 정보를 볼 필요가 없다. 하지만 테스트
라이브러리와 Robot Framework 자체의 문제를 조사할 때 유용하다. 시스템
로그는 기본적으로 생성되지 않지만 선택된 파일에 대한 경로를 포함하는
변수 ``ROBOT_SYSLOG_FILE`` 를 설정하여 활성화 할 수 있다.

..
   A system log has the same `log levels`_ as a normal log file, with the
   exception that instead of `FAIL` it has the `ERROR`
   level. The threshold level to use can be altered using the
   ``ROBOT_SYSLOG_LEVEL`` environment variable like shown in the
   example below.  Possible `unexpected errors and warnings`__ are
   written into the system log in addition to the console and the normal
   log file.

시스템 로그는 정상 로그 파일과 같은 `로그 레벨 <log levels_>`__ 를
가진다. 예외는 `FAIL` 대신 `ERROR` 레벨을 가진다는 것이다. 사용하기
위한 임계 레벨은 아래의 예제에서 보여준 것과 같이
``ROBOT_SYSLOG_LEVEL`` 환경 변수를 사용하여 변경할 수 있다. 가능한
`기대하지 않는 에러와 경고`__ 는 시스템 로그 속에 적혀지고 콘솔과
정상로그파일에도 적혀진다.
   
.. sourcecode:: bash

   #!/bin/bash

   export ROBOT_SYSLOG_FILE=/tmp/syslog.txt
   export ROBOT_SYSLOG_LEVEL=DEBUG

   robot --name Syslog_example path/to/tests

__ `Errors and warnings during execution`_
