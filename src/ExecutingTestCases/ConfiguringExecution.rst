Configuring execution
=====================
This section explains different command line options that can be used
for configuring the `test execution`_ or `post-processing
outputs`_. Options related to generated output files are discussed in
the `next section`__  .

이 절에서는 `test execution`_ 또는 `post-processing
outputs`_ 를 구성하는 데 사용 될수있는 다른 명령행 옵션을 설명한다.
생성 된 출력 파일에 관한 옵션은 `next section`__ 에서 설명한다.

__ `Created outputs`_
__ `Created outputs`_

.. contents::
   :depth: 2
   :local:

Selecting test cases
--------------------

Robot Framework offers several command line options for selecting
which test cases to execute. The same options also work when
post-processing outputs with the ``rebot`` tool.

Robot Framework는 실행할 test case를 선택하는 것에 대한 몇 가지 명령행 옵션을 제공한다.
같은 옵션은 또 ``rebot`` 도구를 사용하여 결과를 후처리할 때 작동한다.

By test suite and test case names
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Test suites and test cases can be selected by their names with the command
line options :option:`--suite (-s)` and :option:`--test (-t)`,
respectively.  Both of these options can be used several times to
select several test suites or cases. Arguments to these options are
case- and space-insensitive, and there can also be `simple patterns`_ matching multiple names.  If both the :option:`--suite` and
:option:`--test` options are used, only test cases in matching suites
with matching names are selected.

test case와  test suite들은 각각 명령행 옵션 :option:`--suite (-s)` 과 :option:`--test (-t)` 으로 선택될 수있다.
이러한 모든 옵션들은 여러 test case와  test suite를 선택하기 위해 여러 번 사용될 수있다.
이러한 옵션의 인자는 공백과 대소문자를 구분하지 않으며, 여러 이름과 매칭하는`simple patterns`_  있을 수 있다.
:option:`--suite` 과 :option:`--test`  이 둘 다 사용된다면, 일치하는 test suite의 test case만이 선택된다. ::

  --test Example
  --test mytest --test yourtest
  --test example*
  --test mysuite.mytest
  --test *.suite.mytest
  --suite example-??
  --suite mysuite --test mytest --test your*

Using the :option:`--suite` option is more or less the same as executing only
the appropriate test case file or directory. One major benefit is the
possibility to select the suite based on its parent suite. The syntax
for this is specifying both the parent and child suite names separated
with a dot. In this case, the possible setup and teardown of the parent
suite are executed.

:option:`--suite` 을 사용하면 적절한 test case 파일과 디렉토리만 수행할때랑 거의 같다.
주요 장점 중 하나는 부모 suite를 기반으로 suite를 선택할수 있다는 것이다.
점으로 구분하여 부모 suite와 자식 suite 둘다 지정한다. 이 경우, 부모 suite에서 가능한 setup, teardown이 수행된다.
::

  --suite parent.child
  --suite myhouse.myhousemusic --test jack*

Selecting individual test cases with the :option:`--test` option is very
practical when creating test cases, but quite limited when running tests
automatically. The :option:`--suite` option can be useful in that
case, but in general, selecting test cases by tag names is more
flexible.
test case를 만들 때 :option:`--test` 으로 각각의 test case를 선택하면 매우 실용적이다.
하지만 test가 자동적으로 수행될 때는 꽤 제한적이다.
:option:`--suite` 옵션은 경우에 유용 할 수 있지만, 일반적으로, 태그 이름에 의해 test case를 선택하는 것이 훨씬 유연하다.



By tag names
~~~~~~~~~~~~

It is possible to include and exclude test cases by tag_ names with the
:option:`--include (-i)` and :option:`--exclude (-e)` options, respectively.
If the :option:`--include` option is used, only test cases having a matching
tag are selected, and with the :option:`--exclude` option test cases having a
matching tag are not. If both are used, only tests with a tag
matching the former option, and not with a tag matching the latter,
are selected.

:option:`--include (-i)` 과 :option:`--exclude (-e)` 옵션으로 tag_ 이름에 의해
test case를 포함하고 제외 할수 있다. :option:`--include` 옵션이 사용된다면, 태그가 일치하는 test case가 선택되고,
:option:`--exclude` 옵션이 사용되면 일치하는 태그가 없는 test case가 선택된다.
둘다 사용되면, 전자에는 일치되고 후자에는 태그가 일치되지 않는 테스트가 선택된다.

::

   --include example
   --exclude not_ready
   --include regression --exclude long_lasting

Both :option:`--include` and :option:`--exclude` can be used several
times to match multiple tags. In that case a test is selected
if it has a tag that matches any included tags, and also has no tag
that matches any excluded tags.

:option:`--include` 와:option:`--exclude` 둘다 다수의 태그와 일치하여 여러번 사용될 수 있다.
이 경우, 포함해야할 태그들를 가지고, 제외해야 할 어떤 태그들도 가지지 않는 테스트가 선택된다.

In addition to specifying a tag to match fully, it is possible to use
`tag patterns`_ where `*` and `?` are wildcards and
`AND`, `OR`, and `NOT` operators can be used for
combining individual tags or patterns together

완전히 일치하는 태그를 지정하는 것 이외에도, where `*` 과 `?` 이 와일드카드인 곳에서 `tag patterns`_ 을 쓰는 것도 가능하다.
그리고, `AND`, `OR`,  `NOT` 연산자들은 태그와 패턴들을 함께 조합하는데 이용된다.
::

   --include feature-4?
   --exclude bug*
   --include fooANDbar
   --exclude xxORyyORzz
   --include fooNOTbar

Selecting test cases by tags is a very flexible mechanism and allows
many interesting possibilities

태그로 test case를 선택하는 것은 매우 유연한 메커니즘이며 많은 흥미로운 가능성이 된다.
:

- A subset of tests to be executed before other tests, often called smoke
  tests, can be tagged with `smoke` and executed with `--include smoke`.

- 다른 시험 전에 수행되는 시험의 subset 인, 일명 스모크 테스트는 `smoke` 로 태그 되고 `--include smoke` 로 시험 수행된다.

- Unfinished test can be committed to version control with a tag such as
  `not_ready` and excluded from the test execution with
  `--exclude not_ready`.

- 미완성된 테스트는 `not_ready` 태그와 함께 커밋될 수있고  `--exclude not_ready` 로 시험 수행에서 제외될 수있다.

- Tests can be tagged with `sprint-<num>`, where
  `<num>` specifies the number of the current sprint, and
  after executing all test cases, a separate report containing only
  the tests for a certain sprint can be generated (for example, `rebot
  --include sprint-42 output.xml`).

- 테스트는 `sprint-<num>` 로 될 수 있다. 여기서 `<NUM>` 은 현재 스프린트의 수를 지정한다.
  모든 test case를 실행 한 후, 특정 sprint를 위한 테스트만 포함하고 있는 별도의 리포트들이 생성된다.
  (for example, `rebot   --include sprint-42 output.xml`).

Re-executing failed test cases
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Command line option :option:`--rerunfailed (-R)` can be used to select all failed
tests from an earlier `output file`_ for re-execution. This option is useful,
for example, if running all tests takes a lot of time and one wants to
iteratively fix failing test cases.

명령행 옵션 :option:`--rerunfailed (-R)` 은 재시험을 위해 이전 `output file`_ 에서 모든 실패된 시험을 선택할 수있다.
이 옵션은 유용하다. 예를 들어 모든 테스트를 실행하는데 많은 시간이 소요되고, 실패하는 테스트를 반복적으로 수정할때 유용하다.
::

  robot tests                             # first execute all tests
  robot --rerunfailed output.xml tests    # then re-execute failing

Behind the scenes this option selects the failed tests as they would have been
selected individually with the :option:`--test` option. It is possible to further
fine-tune the list of selected tests by using :option:`--test`, :option:`--suite`,
:option:`--include` and :option:`--exclude` options.

이 옵션은 뒤에서 :option:`--test` 으로 각각 선택하는 것처럼 실패한 테스트를 선택한다.
:option:`--test`, :option:`--suite`, :option:`--include` , :option:`--exclude` 들을 써서,
선택된 테스트 목록을 미세하게 조정할 수있다.

Using an output not originating from executing the same tests that are run
now causes undefined results. Additionally, it is an error if the output
contains no failed tests. Using a special value `NONE` as the output
is same as not specifying this option at all.

같은 시험을 수행하여 얻은 것이 아닌  출력를 이용하면 확실하지 않은 결과가 나온다.
추가적으로, 출력물이 실패가 아닌 테스트를 포함한다면 이것은 에러다.
출력물로 `NONE` 을 사용하면, 옵션을 전혀 지정하지 않은 것과 같다.

.. tip:: Re-execution results and original results can be `merged together`__
         using the :option:`--merge` command line option.

.. tip:: 재실행한 결과와 원래 결과는 :option:`--merge` 을 써서 `merged together`__  합칠 수 있다.

.. note:: Re-executing failed tests is a new feature in Robot Framework 2.8.
          Prior Robot Framework 2.8.4 the option was named :option:`--runfailed`.
          The old name still works, but it will be removed in the future.

.. note:: 실패된 테스트를 재실행하는 것은 Robot Framework 2.8의 신규 기능이다.
          Robot Framework 2.8.4  이전 버전에서는 :option:`--runfailed` 이었다.
          예전 버전도 동작하고 있지만, 나중에는 삭제 될 것이다.

__ `Merging outputs`_
__ `Merging outputs`_


When no tests match selection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default when no tests match the selection criteria test execution fails
with an error like

기본적으로 선택 기준와 매치 되는 테스트가 없을 때, 시험 수행은 아래 에러를 보여주고 실패한다.
::

    [ ERROR ] Suite 'Example' with includes 'xxx' contains no test cases.

Because no outputs are generated, this behavior can be problematic if tests
are executed and results processed automatically. Luckily a command line
option :option:`--RunEmptySuite` can be used to force the suite to be executed
also in this case. As a result normal outputs are created but show zero
executed tests. The same option can be used also to alter the behavior when
an empty directory or a test case file containing no tests is executed.

생성되는 결과가 없기 때문에, 만약 시험이 자동적으로 수행되고 결과가 처리된다면, 이런 동작은 문제가 될 수있다.
다행히 :option:`--RunEmptySuite` 옵션은 suite가 이런 경우에서도 강제로 수행되도록 한다.
결과적으로 정상 출력이 생성되지만 실행된 테스트를 0으로 표시한다.
빈 디렉토리 또는 test case 파일이 수행되는 테스트를 포함하고 있지 않을때, 동일한 옵션이 동작을 변경하도록 사용될 수도 있다

Similar situation can occur also when processing output files with rebot_.
It is possible that no test match the used filtering criteria or that
the output file contained no tests to begin with. By default executing
``rebot`` fails in these cases, but it has a separate
:option:`--ProcessEmptySuite` option that can be used to alter the behavior.
In practice this option works the same way as :option:`--RunEmptySuite` when
running tests.

rebot_ 으로 출력 파일을 처리할때 비슷한 상황이 발생할 수 있다.
사용된 필터링 기준에 일치되는 테스트가 없거나 또는 출력 파일이 시작할 테스트를 포함하지 않으면 발생할 수 있다.
기본적으로 이런 경우 ``rebot`` 실행이 실패한다. 하지만 별도의 :option:`--ProcessEmptySuite` 옵션은 동작을 변경하는데 사용될 수 있다.
시험이 수행중일때, 실제로 이 옵션은 :option:`--RunEmptySuite` 과 동일한 방식으로 동작한다.

.. note:: :option:`--ProcessEmptySuite` option was added in Robot Framework 2.7.2.
.. note:: :option:`--ProcessEmptySuite` 옵션은 Robot Framework 2.7.2 에서 추가 되었다.

Setting criticality
-------------------

The final result of test execution is determined based on
critical tests. If a single critical test fails, the whole test run is
considered failed. On the other hand, non-critical test cases can
fail and the overall status is still considered passed.

테스트 수행의 최종 결과는 critical 테스트에 근거하여 결정된다.
만약 한개의 critical test가 실패 한다면, 전체 시험 수행이 실패로 간주된다.
반면에 non-critical 테스트는 실패할 수 있고, 전체 상태는 여전히 성공으로 간주된다.

All test cases are considered critical by default, but this can be changed
with the :option:`--critical (-c)` and :option:`--noncritical (-n)`
options. These options specify which tests are critical
based on tags_, similarly as :option:`--include` and
:option:`--exclude` are used to `select tests by tags`__.
If only :option:`--critical` is used, test cases with a
matching tag are critical. If only :option:`--noncritical` is used,
tests without a matching tag are critical. Finally, if both are
used, only test with a critical tag but without a non-critical tag are
critical.

모든 test case는 기본적으로 critical로 간주된다.
하지만 이것은 :option:`--critical (-c)` 과 :option:`--noncritical (-n)` 옵션으로 변경될 수 있다.
이런 옵션들은  `select tests by tags`__ 태그를 기반으로 시험을 선택 하는 :option:`--include` 과 :option:`--exclude` 옵션처럼,
tags_ 를 기반으로 critical 한 시험을 지정할 수 있다.
:option:`--critical` 옵션을 쓴다면, 태그가 일치하는 시험은 critical이다.
:option:`--noncritical` 옵션만 쓴다면, 일치되는 태그가 없는 시험은 critical 이다.
마지막으로 둘다 쓴다면, critical 태그는 있고, non-critical 태그는 없으면 critical 이다.

Both :option:`--critical` and :option:`--noncritical` also support same `tag
patterns`_ as :option:`--include` and :option:`--exclude`. This means that pattern
matching is case, space, and underscore insensitive, `*` and `?`
are supported as wildcards, and `AND`, `OR` and `NOT`
operators can be used to create combined patterns.

:option:`--include` 과 :option:`--exclude` 옵션처럼
:option:`--critical` 와 :option:`--noncritical`  옵션 둘다 `tag patterns`_ 을 지원한다.
이것은 , 패턴 매칭이 대소문자, 공백, underscore를 구분하지 않고, `*` 과 `?`를 wildcard로 지원한다는것을 의미한다.
`AND`, `OR` ,`NOT` 연산자를 조합된 패턴을 생성하는데 사용될 수 있다.
::

  --critical regression
  --noncritical not_ready
  --critical iter-* --critical req-* --noncritical req-6??

The most common use case for setting criticality is having test cases
that are not ready or test features still under development in the
test execution. These tests could also be excluded from the
test execution altogether with the :option:`--exclude` option, but
including them as non-critical tests enables you to see when
they start to pass.

criticality를 설정하는 가장 일반적인 사용 사례는 시험 수행에 대해 준비가 되지 않거나, 테스트 기능이 아직 개발중인 test case를 가지고있는 것이다.
이런 시험들은 :option:`--exclude` 옵션으로 전체 시험 수행에서 제외될 수 있다.
하지만, non-critical 시험으로 이것들을 포함하면, 이 시험들이 패스로 처리되는 것을 확인 할수 있다.

Criticality set when tests are
executed is not stored anywhere. If you want to keep same criticality
when `post-processing outputs`_ with ``rebot``, you need to
use :option:`--critical` and/or :option:`--noncritical` also with it

Criticality set은 시험이 진행되면 저장되지 않는다.
``rebot`` 으로 `post-processing outputs`_ 를 할때, 같은 Criticality set를 유지하고 싶다면
:option:`--critical` , :option:`--noncritical` 을 따로 또는 같이 사용하면 된다.

::

  # Use rebot to create new log and report from the output created during execution
  robot --critical regression --outputdir all my_tests.html
  rebot --name Smoke --include smoke --critical regression --outputdir smoke all/output.xml

  # No need to use --critical/--noncritical when no log or report is created
  robot --log NONE --report NONE my_tests.html
  rebot --critical feature1 output.xml

__ `By tag names`_
__ `By tag names`_


Setting metadata
----------------

Setting the name
~~~~~~~~~~~~~~~~

When Robot Framework parses test data, `test suite names are created
from file and directory names`__. The name of the top-level test suite
can, however, be overridden with the command line option
:option:`--name (-N)`. Underscores in the given name are converted to
spaces automatically, and words in the name capitalized.

Robot Framework는 테스트 데이터를 구문 분석 할 때,
`파일과 디렉토리 이름으로 test suite 이름을 생성한다.`__
최상위 test suite의 이름은 생성될 수있지만, 명령행 옵션 :option:`--name (-N)` 으로 지정할 수있다.
이름에 있는 underscore는 공백으로 자동 변경되고 문자는 대문자로 표시된다.


__ `Test suite name and documentation`_
__ `Test suite name and documentation`_


Setting the documentation
~~~~~~~~~~~~~~~~~~~~~~~~~

In addition to `defining documentation in the test data`__ , documentation
of the top-level suite can be given from the command line with the
option :option:`--doc (-D)`. Underscores in the given documentation
are converted to spaces, and it may contain simple `HTML formatting`_.

`테스트 데이터로 문서를 규정하는 것`__ 외에도,
상위 레벨 suite의 문서는 명령행 옵션 :option:`--doc (-D)` 으로 주어진다.
지정된 문서의 underscore는 공백으로 변환하고 간단한 `HTML formatting`_ 을 포함 할 수있다.

__ `Test suite name and documentation`_
__ `Test suite name and documentation`_

Setting free metadata
~~~~~~~~~~~~~~~~~~~~~

`Free test suite metadata`_ may also be given from the command line with the
option :option:`--metadata (-M)`. The argument must be in the format
`name:value`, where `name` the name of the metadata to set and
`value` is its value. Underscores in the name and value are converted to
spaces, and the latter may contain simple `HTML formatting`_. This option may
be used several times to set multiple metadata.

명령행 옵션 :option:`--metadata (-M)` 으로 `Free test suite metadata`_ 가 제공 될수 있다.
인수의 형식은 `name:value` 이며, `name` 은 설정할 메타데이터의 이름이고, `value` 는 이것의 값이다.
이름과 값의 underscore는 공백으로 변경되며, 값은 `HTML formatting`_ 을 포함한다.
이 옵션은 여러 개의 메타테이터를 설정하기 위해 여러번 쓰일 수있다.


Setting tags
~~~~~~~~~~~~

The command line option :option:`--settag (-G)` can be used to set
the given tag to all executed test cases. This option may be used
several times to set multiple tags.

명령행 옵션 :option:`--settag (-G)` 는 수행되는 모든 test case의 태그를 설정할 수있다.
이 옵션은 여러 태그를 설정하기 위해 여러번 쓰일 수 있다.

.. _module search path:

Configuring where to search libraries and other extensions
----------------------------------------------------------

When Robot Framework imports a `test library`__, `listener`__, or some other
Python based extension, it uses the Python interpreter to import the module
containing the extension from the system. The list of locations where modules
are looked for is called *the module search path*, and its contents can be
configured using different approaches explained in this section.
When importing Java based libraries or other extensions on Jython, Java
classpath is used in addition to the normal module search path.

Robot Framework이 `test library`__, `listener`__ 또는 다른 파이썬 기반의 확장 코드을 가져올 때,
시스템에서 확장 코드을 포함하는 모듈을 가져오기 위해 파이썬 인터프리터(interpreter)를 사용한다.
모듈이 찾는 장소의 리스트를 *모듈 검색 경로(the module search path)* 로 부른다.
그리고 이 콘텐츠는 이번 절에서 설명될 다른 접근방법으로 구성된다.
자바 기반 라이브러리나 Jython 기반의 확장 코드를 가져올 때, 정상 모듈 검색 경로에 자바 classpath가 추가로 사용된다.

Robot Framework uses Python's module search path also when importing `resource
and variable files`_ if the specified path does not match any file directly.

`resource and variable files`_ 을 가져올 때,
지정된 경로에 일치하는 파일이 없으면 Robot Framework는 Python의 모듈 검색 경로를 사용한다.

The module search path being set correctly so that libraries and other
extensions are found is a requirement for successful test execution. If
you need to customize it using approaches explained below, it is often
a good idea to create a custom `start-up script`_.

모듈 검색 경로는 바르게 설정되기 위해서, 라이브러리나 다른 확장코드는 성공적인 테스트 실행을 위한 요구사항이다.
만약 이것을 최적화 하기 위해서, 아래 설명된 방법을 써야한다.
맞춤형 `start-up script`_  시작 스크립트를 생성하는 것은 좋은 아이디어다.

__ `Specifying library to import`_
__ `Specifying library to import`_
__ `Setting listeners`_
__ `Setting listeners`_

Locations automatically in module search path
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Python interpreters have their own standard library as well as a directory
where third party modules are installed automatically in the module search
path. This means that test libraries `packaged using Python's own packaging
system`__ are automatically installed so that they can be imported without
any additional configuration.

Python 인터프리터들은(interpreter) 그들의 표준 라이브러리뿐만 아니라  모듈 검색 경로를 통해 자동으로 설치된 다른 프리터의 모듈의 디렉토리도 가진다.
이것은 `packaged using Python's own packaging system`__ 한 시험 라이브러리가 자동적으로 설치되서, 추가적인 설정없이 불어올수 있다는 것을 의미한다.

__ `Packaging libraries`_
__ `Packaging libraries`_

``PYTHONPATH``, ``JYTHONPATH`` and ``IRONPYTHONPATH``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Python, Jython and IronPython read additional locations to be added to
the module search path from ``PYTHONPATH``, ``JYTHONPATH`` and
``IRONPYTHONPATH`` environment variables, respectively. If you want to
specify more than one location in any of them, you need to separate
the locations with a colon on UNIX-like machines (e.g.
`/opt/libs:$HOME/testlibs`) and with a semicolon on Windows (e.g.
`D:\libs;%HOMEPATH%\testlibs`).

Python, Jython ,IronPython 들은
각각 ``PYTHONPATH``, ``JYTHONPATH`` , ``IRONPYTHONPATH`` 의 환경 변수로 부터,
모듈 검색 경로가 추가된 추가적인 장소를 읽는다.
만약 이것들 중 어떤것에 대해 좀더 자세한 정보를 원한다면,
유닉스 계열에서는 콜론(colon)으로(e.g. `/opt/libs:$HOME/testlibs`),
윈도우에서는 세미콜론(semicolon)으로 얻을 수 있다(e.g.`D:\libs;%HOMEPATH%\testlibs`).

Environment variables can be configured permanently system wide or so that
they affect only a certain user. Alternatively they can be set temporarily
before running a command, something that works extremely well in custom
`start-up scripts`_.

환경변수는 영구적으로 시스템 전역에 설정될수 있고 또는 특정사용자에게만 영향을 줄 수도 있다.
대안적으로 명령 수행 전에, 사용자 정의 `start-up scripts`_ 에 매우 잘 작동하는 것으로 그것은 임시적으로 설정될 수있다.

.. note:: Prior to Robot Framework 2.9, contents of ``PYTHONPATH`` environment
          variable were added to the module search path by the framework itself
          when running on Jython and IronPython. Nowadays that is not done
          anymore and ``JYTHONPATH`` and ``IRONPYTHONPATH`` must be used with
          these interpreters.

.. note:: Robot Framework 2.9 이전에는,
          Jython 과 IronPython으로 수행될 때, 환경변수 ``PYTHONPATH``의 내용은 framework에 의해 모듈 검색 경로가 더해졌다.
          현재는, 더이상 그렇게 동작하지 않는다. ``JYTHONPATH`` 과 ``IRONPYTHONPATH`` 는 이런 인터프리터에 의해 쓰여진다.




Using `--pythonpath` option
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Robot Framework has a separate command line option :option:`--pythonpath (-P)`
for adding locations to the module search path. Although the option name has
the word Python in it, it works also on Jython and IronPython.

Robot Framework는 모듈 검색 경로에 장소를 더하기 위해 명령행 옵션 :option:`--pythonpath (-P)` 를 쓴다.
비록 이 옵션 이름은 Python 을 포함하지만 Jython 과 IronPython에서도 동작한다.

Multiple locations can be given by separating them with a colon, regardless
the operating system, or by using this option several times. The given path
can also be a glob pattern matching multiple paths, but then it typically
needs to be escaped__.

여러개의 장소들이 운영체제에 상관없이 장소들을 colon으로 구분하거나, 옵션을 여러번 사용하여 제공될 수있다.
주어진 경로는 전역적인 패턴 매칭 다중 경로가 될 수있지만 전형적으로 이스케이프( escaped__ ) 가 필요하다.

__ `Escaping complicated characters`_
__ `Escaping complicated characters`_

Examples::

   --pythonpath libs
   --pythonpath /opt/testlibs:mylibs.zip:yourlibs
   --pythonpath mylib.jar --pythonpath lib/STAR.jar --escape star:STAR


Configuring `sys.path` programmatically
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Python interpreters store the module search path they use as a list of strings
in `sys.path <https://docs.python.org/2/library/sys.html#sys.path>`__
attribute. This list can be updated dynamically during execution, and changes
are taken into account next time when something is imported.

파이썬 인터프리터(interpreters)는
그들이 쓰는 `sys.path <https://docs.python.org/2/library/sys.html#sys.path>`__ 속성의 문자열(strings) 리스트 로서
모듈 검사 경로를 저장한다.
이 리스트는 수행하는 동안 다이나믹하게 업데이트된다.
그리고 어떤것이 가져와 질때, 다음번에 변경사항들은 계정으로 반영된다.**


Java classpath
~~~~~~~~~~~~~~

When libraries implemented in Java are imported with Jython, they can be
either in Jython's normal module search path or in `Java classpath`__. The most
common way to alter classpath is setting the ``CLASSPATH`` environment variable
similarly as ``PYTHONPATH``, ``JYTHONPATH`` or ``IRONPYTHONPATH``.
Alternatively it is possible to use Java's :option:`-cp` command line option.
This option is not exposed to the ``robot`` `runner script`_, but it is
possible to use it with Jython by adding :option:`-J` prefix like
`jython -J-cp example.jar -m robot.run tests.robot`.

자바로 구현된 라이브러리를 Jython으로 가져 오면,
이것은 Jython의 정상 모듈 검색 경로 또는 `Java classpath`__ 안에 있을 수 있다.
클래스 경로를 변경하는 가장 일반적인 방법은 ``PYTHONPATH``, ``JYTHONPATH`` , ``IRONPYTHONPATH`` 들같은
환경변수 ``CLASSPATH`` 을 설정하는 것이다.
대안적으로 명령행 옵션 :option:`-cp` 을 이용할 수 있다.
이런 옵션들은 ``robot`` `runner script`_ 에 노출되지 않지만,
`jython -J-cp example.jar -m robot.run tests.robot` 같은 접두사를 더하므로서
Jython 과 함께 이것을 쓰는 것이 가능하다.

When using the standalone JAR distribution, the classpath has to be set a
bit differently, due to the fact that `java -jar` command does support
the ``CLASSPATH`` environment variable nor the :option:`-cp` option. There are
two different ways to configure the classpath

standalone JAR distribution를 사용하는 경우,
`java -jar` 의 명령은 ``CLASSPATH`` 환경변수와 the :option:`-cp` option를 지원하지 않기 때문에
classpath는 약간 다르게 설정될 수 있다.
클래스 경로를 구성하는 두 가지 방법이 있다.

::

  java -cp lib/testlibrary.jar:lib/app.jar:robotframework-2.9.jar org.robotframework.RobotFramework tests.robot
  java -Xbootclasspath/a:lib/testlibrary.jar:lib/app.jar -jar robotframework-2.9.jar tests.robot

__ https://docs.oracle.com/javase/8/docs/technotes/tools/findingclasses.html
__ https://docs.oracle.com/javase/8/docs/technotes/tools/findingclasses.html


Setting variables
-----------------

Variables_ can be set from the command line either individually__
using the :option:`--variable (-v)` option or through `variable files`_
with the :option:`--variablefile (-V)` option. Variables and variable
files are explained in separate chapters, but the following examples
illustrate how to use these options

Variables_ 변수는 individually__ 각각
:option:`--variable (-v)` 옵션을 쓰거나 :option:`--variablefile (-V)` 으로  `variable files`_ 을 통해
설정될 수 있다.
변수와 변수 파일들은 분리된 절로 설명되어 있지만, 아래 예는 옵션을 어떻게 쓰는지 설명한다.


::

  --variable name:value
  --variable OS:Linux --variable IP:10.0.0.42
  --variablefile path/to/variables.py
  --variablefile myvars.py:possible:arguments:here
  --variable ENVIRONMENT:Windows --variablefile c:\resources\windows.py

__ `Setting variables in command line`_
__ `Setting variables in command line`_




Dry run
-------

Robot Framework supports so called *dry run* mode where the tests are
run normally otherwise, but the keywords coming from the test libraries
are not executed at all. The dry run mode can be used to validate the
test data; if the dry run passes, the data should be syntactically
correct. This mode is triggered using option :option:`--dryrun`.

Robot Framework는 테스트가 정상적으로 수행되는 *dry run* 모드를 지원한다. 테스트가 정상적으로 수행되지만
시험 라이브러리에서 불러진 키워드들은 전혀 수행 되지 않는다.
dry run 모드는 테스트 데이터를 검증하는데 사용될 수 있다.
모든 dry run 이 성공한다면, 데이터는 문법적으로 정확해야한다.
이 모드는 :option:`--dryrun` 을 써서 트리거된다.

The dry run execution may fail for following reasons:
dry run 수행이 실패하는 이유 :

  * Using keywords that are not found.
  * Using keywords with wrong number of arguments.
  * Using user keywords that have invalid syntax.

  * 사용중인 키워드가 발견되지 않음.
  * 사용중인 키워드가 잘못된 인자를 갖고 있음.
  * 사용중인 유저 키워드가 잘못된 문법을 사용함.

In addition to these failures, normal `execution errors`__ are shown,
for example, when test library or resource file imports cannot be
resolved.

추가적으로 이러한 실패들은, 보통 `execution errors`__  에 기술된다.
예를 들어 시험 라이브러리와 리소스 파일을 가져오는 것이 안될 때이다.

.. note:: The dry run mode does not validate variables. This
          limitation may be lifted in the future releases.

.. note:: dry run 모드는 변수를 검증하지 않는다.
          이 제한사항은 추후 릴리즈에서 해결될 것이다.

__ `Errors and warnings during execution`_
__ `Errors and warnings during execution`_


Randomizing execution order
---------------------------

The test execution order can be randomized using option
:option:`--randomize <what>[:<seed>]`, where `<what>` is one of the following:

테스트 실행 순서는 :option:`--randomize <what>[:<seed>]` 옵션을 사용하면 랜덤으로 정해진다.
`<what>` 은 아래의 것중 하나이다. :

`tests`
    Test cases inside each test suite are executed in random order.

`tests`
    각 test suite 안에 있는 Test case는 랜덤한 순서로 수행된다.

`suites`
    All test suites are executed in a random order, but test cases inside
    suites are run in the order they are defined.

`suites`
    모든 test suite 는 랜덤한 순서로 수행된다. 하지만 suite안의 test case들은 그들이 정의된 순서로 수행된다.

`all`
    Both test cases and test suites are executed in a random order.

`all`
    test case들과 test suite들은 랜덤한 순서로 수행된다.

`none`
    Neither execution order of test nor suites is randomized.
    This value can be used to override the earlier value set with
    :option:`--randomize`.

`none`
    test case들과 suite들 모두 랜덤하지 않다.
    이 값은 :option:`--randomize` 옵션에 의해서 더 쉬운 값으로 재설정될 수 있다.

Starting from Robot Framework 2.8.5, it is possible to give a custom seed
to initialize the random generator. This is useful if you want to re-run tests
using the same order as earlier. The seed is given as part of the value for
:option:`--randomize` in format `<what>:<seed>` and it must be an integer.
If no seed is given, it is generated randomly. The executed top level test
suite automatically gets metadata__ named :name:`Randomized` that tells both
what was randomized and what seed was used.

Robot Framework 2.8.5 부터,
random generator 를 초기화 하는데 custom seed를 설정하는 것이 가능하다.
이전과 같은 순서로 재시험을 실행 하려는 경우 유용하다.
이 seed 는 `<what>:<seed>` 형식으로 :option:`--randomize` 옵션으로 값을 줄 수 있으며,
이 값은 정수이여야 한다.
seed를 주지 않으면, 랜덤으로 생성된다.
자동적으로 수행되는 상위 레벨 test suite 는 :name:`Randomized` 으로 지정된 metadata__  를 얻는다.
이 함수는 사용했었던 seed 값과 랜덤한 값을 알려준다.

Examples::

    robot --randomize tests my_test.txt
    robot --randomize all:12345 path/to/tests


__ `Free test suite metadata`_
__ `Free test suite metadata`_

Programmatic modification of test data
--------------------------------------

If the provided built-in features to modify test data before execution
are not enough, Robot Framework 2.9 and newer provide a possible to do
custom modifications programmatically. This is accomplished by creating
a model modifier and activating it using the :option:`--prerunmodifier`
option.

제공되는 built-in 기능들로 수행전에 test data를 수정하는 것은 충분하지 않다.
Robot Framework 2.9 이후부터는 프로그램적인 사용자 지정 수정을 제공한다.
이는 :option:`--prerunmodifier` 옵션을 써서 모델 변경자를 만들고 활성화하여 수행된다.

Model modifiers should be implemented as visitors that can traverse through
the executable test suite structure and modify it as needed. The visitor
interface is explained as part of the `Robot Framework API documentation`__ ,
and the example below ought to give an idea of how it can be used and how
powerful this functionality is.

모델 변경자는 실행가능한 test suite structure를 통과하고 필요에 따라 수정할 수있는 방문자로 구현되어야 한다.**
방문자 인터페이스는 `Robot Framework API documentation`__ 의 일환으로 설명된다.
그리고 아래 예를 들어 어떻게 사용할 수 있고 이것이 기능적으로 얼마나 강력한지에 대한 아이디어를 제공한다.



.. sourcecode:: python

   ../api/code_examples/select_every_xth_test.py

When a model modifier is taken into use on the command line using the
:option:`--prerunmodifier` option, it can be specified either as a name of
the modifier class or a path to the modifier file. If the modifier is given
as a class name, the module containing the class must be in the `module search
path`_, and if the module name is different than the class name, the given
name must include both like `module.ModifierClass`. If the modifier is given
as a path, the class name must be same as the file name. For most parts this
works exactly like when `specifying a test library to import`__.

:option:`--prerunmodifier` 옵션을 써서 모델 변경자가 명령행에 쓰이는 경우, 변경자 클래스의 이름 또는 변경자 파일 경로로서 지정될 수 있다.
만약 변경자가 클래스의 이름으로 주어진 경우, 클래스를 포함하는 모듈은 `module search
path`_ 안에 있어야 한다. 그리고 만약 모듈 이름이 클래스 이름과 다르다면 주어진 이름은 `module.ModifierClass` 처럼 포함해야한다.
클래스 경로로서 지정되었을 경우, 클래스 이름은 파일명과 동일해야한다.
이 동작의 대부분은 `specifying a test library to import`__ 가져올 테스트 라이브러리를 지정할 때 처럼 작동한다.


If a modifier requires arguments, like the example above does, they can be
specified after the modifier name or path using either a colon (`:`) or a
semicolon (`;`) as a separator. If both are used in the value, the one first
is considered the actual separator.

변경자가 인자를 요구한다면, 위에 예처럼,
변경자 이름이나, 콜론(`:`) 또는
세미콜론 (`;`)을 구분자로 사용한 경로 뒤에 지정된다.
만약 둘다 값에 쓰인다면, 먼저 쓰인 하나가 실제 구분자로 여겨진다.

For example, if the above model modifier would be in a file
:file:`SelectEveryXthTest.py`, it could be used like this

예를 들어, 만약 위의 모델 변경자가 파일안 :file:`SelectEveryXthTest.py` 에 있다면,
아래 처럼 쓰여질 것이다.

::

    # Specify the modifier as a path. Run every second test.
    robot --prerunmodifier path/to/SelectEveryXthTest.py:2 tests.robot

    # Specify the modifier as a name. Run every third test, starting from the second.
    # SelectEveryXthTest.py must be in the module search path.
    robot --prerunmodifier SelectEveryXthTest:3:1 tests.robot

If more than one model modifier is needed, they can be specified by using
the :option:`--prerunmodifier` option multiple times. If similar modifying
is needed before creating results, `programmatic modification of results`_
can be enabled using the :option:`--prerebotmodifier` option.

만약 한개 이상의 모델 변경자가 필요하다면, :option:`--prerunmodifier` 옵션을 여러번 쓰면 된다.
유사한 변경이 결과를 생성하기 전에 필요하다면, `programmatic modification of results`_  결과의 프로그램적인 변경은
:option:`--prerebotmodifier` 으로 할 수 있다.



__ https://robot-framework.readthedocs.org/en/latest/autodoc/robot.model.html#module-robot.model.visitor
__ https://robot-framework.readthedocs.org/en/latest/autodoc/robot.model.html#module-robot.model.visitor
__ `Specifying library to import`_
__ `Specifying library to import`_

Controlling console output
--------------------------

There are various command line options to control how test execution is
reported on the console.


테스트 실행이 콘솔에 보고하는 방법을 제어하는 다양한 명령행 옵션이 있다.

Console output type
~~~~~~~~~~~~~~~~~~~

The overall console output type is set with the :option:`--console` option.
It supports the following case-insensitive values:

전체 콘솔 출력 유형은 :option:`--console` 옵션으로 설정된다. 이것은 대소문자를 구분하지 않는다.

`verbose`
    Every test suite and test case is reported individually. This is
    the default.

`verbose`
    모든 test suite 와 test case는 각각 보고된다. 이것이 기본값이다.

`dotted`
    Only show `.` for passed test, `f` for failed non-critical tests, `F`
    for failed critical tests, and `x` for tests which are skipped because
    `test execution exit`__. Failed critical tests are listed separately
    after execution. This output type makes it easy to see are there any
    failures during execution even if there would be a lot of tests.

`dotted`
    `.`는 성공한 테스트에 대해 ,`f` 는 실패한 non-critical test에 대해, `F`
    는 실패한 critical test에 대해, 그리고 `x` 는 `test execution exit`__ 때문에 건너뛴 시험에 대해서만 쓰인다.
    실패한 critical test들은 수행 후에 분리되서 리스트된다 .
    이 출력 유형은 많은 시험이 남아있고, 시험이 실행 중에 어떤 실패가 있는지 보기 쉽도록 한다.

`quiet`
    No output except for `errors and warnings`_.

`quiet`
    `errors and warnings`_ 에러와 워닝을 제외하고 출력되지 않는다.

`none`
    No output whatsoever. Useful when creating a custom output using,
    for example, listeners_.

`none`
    어떠한 것도 출력하지 않는다. 사용자 지정하여 출력을 만들때 유용하다.예를 들어, listeners_을 만들 때 유용하다.

__ `Stopping test execution gracefully`_
__ `Stopping test execution gracefully`_

Separate convenience options :option:`--dotted (-.)` and :option:`--quiet`
are shortcuts for `--console dotted` and `--console quiet`, respectively.

별도의 편리한 옵션인 :option:`--dotted (-.)` 과 :option:`--quiet` 은 각각  `--console dotted` 과 `--console quiet`
의 단축키이다.
Examples::

    robot --console quiet tests.robot
    robot --dotted tests.robot

.. note:: :option:`--console`, :option:`--dotted` and :option:`--quiet`
          are new options in Robot Framework 2.9. Prior to that the output
          was always the same as in the current `verbose` mode.

.. note:: :option:`--console`, :option:`--dotted` , :option:`--quiet`은
          Robot Framework 2.9의 새로운 옵션이다. 이전 버전에서의 결과는 항상 현재 `verbose` 모드와 같다.


Console width
~~~~~~~~~~~~~

The width of the test execution output in the console can be set using
the option :option:`--consolewidth (-W)`. The default width is 78 characters.

콘솔에서 테스트 실행 출력의 폭은 :option:`--consolewidth (-W)` 으로 정해진다.
기본 너비는 78 이다.

.. tip:: On many UNIX-like machines you can use handy `$COLUMNS`
         environment variable like `--consolewidth $COLUMNS`.

.. tip:: 많은 UNIX 계열 장비에서 `--consolewidth $COLUMNS` 처럼 환경 변수`$COLUMNS`를 설정할수 있다.

.. note:: Prior to Robot Framework 2.9 this functionality was enabled with
          :option:`--monitorwidth` option that is nowadays deprecated.
          The short option :option:`-W` works the same way in all versions.

.. note:: Robot Framework 2.9 이전에서는 :option:`--monitorwidth` 이 동작했지만 지금은 동작하지 않는다.
          짧은 옵션 :option:`-W` 은 모든 버전에서 같은 방법으로 동작한다.

Console colors
~~~~~~~~~~~~~~

The :option:`--consolecolors (-C)` option is used to control whether
colors should be used in the console output. Colors are implemented
using `ANSI colors`__ except on Windows where, by default, Windows
APIs are used instead. Accessing these APIs from Jython is not possible,
and as a result colors do not work with Jython on Windows.


:option:`--consolecolors (-C)` 옵션은 콘솔 출력에 색상을 사용할 것인지 제어하기 위해 사용된다.
색상은 윈도우를 제외하고  `ANSI colors`__  를 사용하여 수행된다.
기본적으로 윈도우는 윈도우 API가 대신 사용된다.
Jython에서 API에 접근하는 것은 불가능 하고, 윈도우에서 jython으로 구현한 색상은 동작하지 않는다.

This option supports the following case-insensitive values:
옵션은 대소문자를 구분하지 않는다.:

`auto`
    Colors are enabled when outputs are written into the console, but not
    when they are redirected into a file or elsewhere. This is the default.

`auto`
    결과가 콘솔에 출력될때 색상이 쓰이지만, 이것이 파일이나 다른곳으로 redirection하면 안된다. 이값이 기본값이다.


`on`
    Colors are used also when outputs are redirected. Does not work on Windows.

`on`
    색상은 결과가 redirection 될때 또한 쓰인다. 하지만 윈도우에서는 동작하지 않는다.


`ansi`
    Same as `on` but uses ANSI colors also on Windows. Useful, for example,
    when redirecting output to a program that understands ANSI colors.
    New in Robot Framework 2.7.5.


`ansi`
    `on` 과 같지만, 윈도우에서도 ANSI 색상을 쓴다. 예를 들어,
    결과를 ANSI 색상을 이해하는 프로그램으로 결과를 redirecting 할때 유용하다.
    이것은 Robot Framework 2.7.5 의 신규기능이다.


`off`
    Colors are disabled.


`off`
    색상을 안쓴다.

.. note:: Prior to Robot Framework 2.9 this functionality was enabled with
          :option:`--monitorcolors` option that is nowadays deprecated.
          The short option :option:`-C` works the same way in all versions.

.. note::  Robot Framework 2.9 이전버전에서는 이 기능은 :option:`--monitorcolors` 이 동작되었고, 지금은 동작하지 않는다.
            단축키 option:`-C` 은 모든 버전에서 같은 방법으로 동작한다.

__ http://en.wikipedia.org/wiki/ANSI_escape_code
__ http://en.wikipedia.org/wiki/ANSI_escape_code

Console markers
~~~~~~~~~~~~~~~

Starting from Robot Framework 2.7, special markers `.` (success) and
`F` (failure) are shown on the console when using the `verbose output`__
and top level keywords in test cases end. The markers allow following
the test execution in high level, and they are erased when test cases end.

Robot Framework 2.7 부터, `verbose output`__ 을 쓸때나 test case 끝에 있는 상위 레벨 키워드를 쓸 때,
특별한 마커인 `.` (성공) 과 `F` (실패) 는 콘솔에 출력된다.
마커는 높은 수준의 테스트 실행을 허용하고 test case가 끝나면 삭제된다.

Starting from Robot Framework 2.7.4, it is possible to configure when markers
are used with :option:`--consolemarkers (-K)` option. It supports the following
case-insensitive values:

Robot Framework 2.7.4 부터, 마커를 :option:`--consolemarkers (-K)` 옵션과 함께 쓸 때, 구성할 수 있다.
이것은 대소문자를 구분하지 않는다. :


`auto`
    Markers are enabled when the standard output is written into the console,
    but not when it is redirected into a file or elsewhere. This is the default.


`auto`
    표준 출력이 콘솔에 쓰여질때 마커가 활성화 된다. 그러나 파일이나 다른곳으로 redirection 되면 그렇지않다.
    이것이 기본값이다.

`on`
    Markers are always used.

`on`
    마커가 항상 쓰인다.

`off`
    Markers are disabled.

`off`
    마커가 동작하지 않는다.

.. note:: Prior to Robot Framework 2.9 this functionality was enabled with
          :option:`--monitormarkers` option that is nowadays deprecated.
          The short option :option:`-K` works the same way in all versions.


.. note::  Robot Framework 2.9 이후에 이 기능은 :option:`--monitormarkers` 옵션과 함께 활성화되고, 지금은 동작하지 않는다.
           단축키 :option:`-K` 는 모든 버전에서 같은 동작을 한다.

__ `Console output type`_
__ `Console output type`_

Setting listeners
-----------------

Listeners_ can be used to monitor the test execution. When they are taken into
use from the command line, they are specified using the :option:`--listener`
command line option. The value can either be a path to a listener or
a listener name. See the `Using listener interface`_ section for more details
about importing listeners and using them in general.

Listeners_ 는 테스트 실행을 모니터링하기 위해 사용될 수있다.
명령행으로부터 리스너가 사용될 때, 명령행 옵션 :option:`--listener` 을 사용하여 지정된다.
값은 Listener의 이름이거나 경로일 수있다.
`Using listener interface`_ 절을 보면 listerner를 가져오거나 일반적인 사용에 대해 좀 더 자세하게 알수있다.
