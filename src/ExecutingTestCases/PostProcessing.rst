.. _rebot:

Post-processing outputs
=======================

`XML output files`_ that are generated during the test execution can be
post-processed afterwards by the ``rebot`` tool, which is an integral
part of Robot Framework. It is used automatically when test
reports and logs are generated during the test execution, and using it
separately allows creating custom reports and logs as well as combining
and merging results.

테스트 수행 과정 중에 생성된 `XML output files`_ 은 로봇 프레임워크에 내장된 ``rebot`` 툴에 의해 수행 후의 포스트 프로세스가 될 수 있다.
테스트 리포트와 로그가 테스트 수행중에 생성되었을 때 자동으로 사용된다.
독립적으로 사용하는 것은 커스텀화된 리포트와 로그를 생성할 수 있게 하고 결과를 결합하고 취합할 수 있다.

.. contents::
   :depth: 2
   :local:

Using ``rebot`` tool
--------------------

Synopsis
~~~~~~~~

::

    rebot|jyrebot|ipyrebot [options] robot_outputs
    python|jython|ipy -m robot.rebot [options] robot_outputs
    python|jython|ipy path/to/robot/rebot.py [options] robot_outputs
    java -jar robotframework.jar rebot [options] robot_outputs

``rebot`` `runner script`_ runs on Python_ but there are also ``jyrebot``
and ``ipyrebot`` `runner scripts`_ that run on Jython_ and IronPython_, respectively.
Using ``rebot`` is recommended when it is available because it is considerable
faster than the alternatives. In addition to using these scripts, it is possible to use
``robot.rebot`` `entry point`_ either as a module or a script using
any interpreter, or use the `standalone JAR distribution`_.

``rebot`` `runner script`_ 은 Python_ 에서 수행되고 Jython_ 에서 수행되는 ``jyrebot``
와 IronPython_ 에서 수행되는 ``ipyrebot`` `runner scripts`_ 가 있다.
가능하다면 ``rebot`` 를 사용하는 것을 추천한다. 왜냐하면 다른것보다 상당히 빠르기 때문이다.
이 스크립트를 사용하는것뿐만 아니라, ``robot.rebot`` `entry point`_ 번역을 사용하는 스크립트나 모듈
혹은 `standalone JAR distribution`_ 을 사용할 수 있다.

Specifying options and arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The basic syntax for using ``rebot`` is exactly the same as when
`starting test execution`_ and also most of the command line options are
identical. The main difference is that arguments to ``rebot`` are
`XML output files`_ instead of test data files or directories.

``rebot`` 를 사용하는 기본 문법은 정확히 `starting test execution`_ 할 때와 비슷하다. 그리고 대부분의 커맨드 라인 옵션이 동일하다.
주된 차이점은 ``rebot`` 로의 전달인자는 테스트 데이터 파일이나 디렉토리 대신에 `XML output files`_ 가 사용된다는 것이다.

Return codes with ``rebot``
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Return codes from ``rebot`` are exactly same as when `running tests`__.

__ `Return codes`_

``rebot`` 로 부터의 리턴 코드는 정확히 `running tests`__ 일때와 같다.

__ `Return codes`_

Creating different reports and logs
-----------------------------------

You can use ``rebot`` for creating the same reports and logs that
are created automatically during the test execution. Of course, it is
not sensible to create the exactly same files, but, for example,
having one report with all test cases and another with only some
subset of tests can be useful

테스트 수행중에 자동으로 생성되는 동일한 리포트와 로그를 생성하기 위해 ``rebot`` 를 사용할 수 있다.
물론, 완전히 같은 파일을 만들 수는 없다.
하지만 예를들어, 모든 테스트 케이스와 같은 것의 리포트를 출력하기 위해서 테스트의 서브셋만 수행하는 것은 유용하다::

   rebot output.xml
   rebot path/to/output_file.xml
   rebot --include smoke --name Smoke_Tests c:\results\output.xml

Another common usage is creating only the output file when running tests
(log and report generation can be disabled with  `--log NONE
--report NONE`) and generating logs and reports later. Tests can,
for example, be executed on different environments, output files collected
to a central place, and reports and logs created there. This approach can
also work very well if generating reports and logs takes a lot of time when
running tests on Jython. Disabling log and report generation and generating
them later with ``rebot`` can save a lot of time and use less memory.

다른 공통 사용은 테스트 수행중에 아웃풋 파일만 만드는 것이다.
(`--log NONE --report NONE` 를 이용하여 로그와 리포트 생성을 비활성화 시킬 수 있다)
그리고 로그와 리포트는 나중에 만든다.
예를들어, 테스트는 다른 환경에서 수행될 수 있다. 아웃풋 파일은 중앙에서 수집되고, 리포트와 로그는 그곳에서 생성될 수 있다.
Jython에서 테스트 수행할때, 만일 로그와 리포트를 생성하는데 많은 시간이 걸리는 경우 이런 접근은 매우 좋다.
로그와 리포트 생성을 비활성화 하고, ``rebot`` 으로 나중에 생성되게 하는것은 시간을 많이 단축시켜주고, 메모리 소모가 적다.

Combining outputs
-----------------

An important feature in ``rebot`` is its ability to combine
outputs from different test execution rounds. This capability allows,
for example, running the same test cases on different environments and
generating an overall report from all outputs. Combining outputs is
extremely easy, all that needs to be done is giving several output
files as arguments

``rebot``  에서 중요한 특징은 다른 시험 수행 결과들과 아웃풋을 결합할 수 있다는 것이다.
예를들어, 이 기능은 같은 테스트 케이스를 다른 환경에서 수행하고 모든 결과에 대한 전반적인 리포트를 생성해준다.
아웃풋을 결합하는 것은 매우 쉽고, 전달인자 처럼 아웃풋 파일만이 필요하다::

   rebot output1.xml output2.xml
   rebot outputs/*.xml

When outputs are combined, a new top-level test suite is created so
that test suites in the given output files are its child suites. This
works the same way when `multiple test data files or directories are
executed`__, and also in this case the name of the top-level test
suite is created by joining child suite names with an ampersand (&)
and spaces. These automatically generated names are not that good, and
it is often a good idea to use :option:`--name` to give a more
meaningful name

결과가 결합되었을때, 새로운 top-level 테스트 스위트가 생성되어, 주어진 아웃풋 파일의 테스트 스위트는 이것의 차일드 스위트가 된다.
이것은 `multiple test data files or directories are executed`__ 와 같은 방법으로 동작한다.
그리고 top-level 테스트 스위트의 이름은 차일드 스위트 이름을 &와 공백으로 결합한 이름이 된다.
자동으로 생성되는 이름은 썩 좋지 않다. :option:`--name` 를 이용하여 더욱 의미있는 이름을 만들어 주는 것이 좋다::

   rebot --name Browser_Compatibility firefox.xml opera.xml safari.xml ie.xml
   rebot --include smoke --name Smoke_Tests c:\results\*.xml

__ `Specifying test data to be executed`_

__ `Specifying test data to be executed`_

Merging outputs
---------------

If same tests are re-executed or a single test suite executed in pieces,
combining results like discussed above creates an unnecessary top-level
test suite. In these cases it is typically better to merge results instead.
Merging is done by using :option:`--merge` option which changes the way how
``rebot`` combines two or more output files. This option itself takes no
arguments and all other command line options can be used with it normally

만일 같은 테스트 케이스가 다시 수행되거나, 단일 테스트 스위트가 여러번에 나뉘어 수행되는 경우,
위에서 이야기한 것 처럼 결과를 취합하는 것은 불필요한 top-level 테스트 스위트를 생성한다.
이런 경우 보통 결과를 결합하기가 쉽다.
결합하는 것은 ``rebot`` 이 두개 이상의 결과 파일을 바꿔주는 :option:`--merge` 옵션을 이용하여 수행할 수 있다.
이 옵션은 전달인자를 갖지 않고, 모든 커맨드 라인 옵션은 그대로 사용 가능하다::

   rebot --merge --name Example --critical regression original.xml merged.xml

How merging works in practice is explained in the following sections discussing
its two main use cases.

실제로 어떻게 결합할 것인지 두개의 메인 유즈 케이스를 다음 섹션에서 논의할 것이다.

Merging re-executed tests
~~~~~~~~~~~~~~~~~~~~~~~~~

There is often a need to re-execute a subset of tests, for example, after
fixing a bug in the system under test or in the tests themselves. This can be
accomplished by `selecting test cases`_ by names (:option:`--test` and
:option:`--suite` options), tags (:option:`--include` and :option:`--exclude`),
or by previous status (:option:`--rerunfailed`).

종종 테스트의 일부분을 다시 수행해야할 때가 있다. 예를들어, 테스트 하의 시스템에서나, 테스트 자체에서 버그를 수정한 후에.
이름(:option:`--test` 그리고 :option:`--suite` options), 태그(:option:`--include` 그리고 :option:`--exclude`),
이전의 상태(:option:`--rerunfailed`)를 이용해서 `selecting test cases`_ 해서 가능하다.

Combining re-execution results with the original results using the default
`combining outputs`_ approach does not work too well. The main problem is
that you get separate test suites and possibly already fixed failures are
also shown. In this situation it is better to use :option:`--merge (-R)`
option to tell ``rebot`` to merge the results instead. In practice this
means that tests from the latter test runs replace tests in the original.
The usage is best illustrated by a practical example using
:option:`--rerunfailed` and :option:`--merge` together

디폴트 `combining outputs`_ 를 이용해서 재수행한 결과와 원본 결과를 결합하는것이 잘 되지 않을 수도 있다.
주요한 문제는 분리된 테스트 스위트와 이미 고쳐진 실패들이 보여진다. 이런 상황에서 결과들을 취합하는 대신
``rebot`` 을 말하기 위해 :option:`--merge (-R)` 옵션을 사용하는 것이 낫다.
실제로 이것은 마지막 수행된 테스트가 이전에 수행된 테스트를 대체한다는 것을 의미한다.
:option:`--rerunfailed` 와 :option:`--merge` 를 함께 사용하는 실제 예제를 통해 설명한다::

  robot --output original.xml tests                          # first execute all tests
  robot --rerunfailed original.xml --output rerun.xml tests  # then re-execute failing
  rebot --merge original.xml rerun.xml                       # finally merge results

The message of the merged tests contains a note that results have been
replaced. The message also shows the old status and message of the test.

취합된 테스트의 메시지는 결과가 대체되었음을 알리는 주석을 포함한다.
또한 이 메시지는 지난 상태와 테스트의 메시지를 보여준다.

Merged results must always have same top-level test suite. Tests and suites
in merged outputs that are not found from the original output are added into
the resulting output. How this works in practice is discussed in the next
section.

취합된 결과는 항상 동일한 top-level 테스트 스위트를 가져야 한다.
취합된 아웃풋의 테스트와 스위트는 원본 아웃풋에서는 찾을 수 없는 것이 최종 결과에 추가된다.
실제로 어떻게 작동하는지는 다음 섹션에서 다룬다.

.. note:: Merging re-executed results is a new feature in Robot Framework 2.8.4.
          Prior to Robot Framework 2.8.6 new tests or suites in merged outputs
          were skipped and merging was done using nowadays deprecated
          :option:`--rerunmerge` option.

.. note:: 재구동한 결과를 취합한것은 로봇 프레임워크 2.8.4부터 지원하는 새 기능이다.
          로봇 프레임워크 2.8.6 이전에는 취합된 아웃풋의 새 테스트와 스위트는 뛰어넘기하고,
          취합은 곧 사라질 :option:`--rerunmerge` 옵션에 의해서 오늘날에 사용되고 있다.

Merging suites executed in pieces
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Another important use case for the :option:`--merge` option is merging results
got when running a test suite in pieces using, for example, :option:`--include`
and :option:`--exclude` options

:option:`--merge` 옵션의 다른 중요한 유즈케이스는 :option:`--include` 와 :option:`--exclude` 옵션을 이용하여
테스트 스위트가 부분별로 수행될 때 얻은 결과를 취합한다::

    robot --include smoke --output smoke.xml tests   # first run some tests
    robot --exclude smoke --output others.xml tests  # then run others
    rebot --merge smoke.xml others.xml               # finally merge results

When merging outputs like this, the resulting output contains all tests and
suites found from all given output files. If some test is found from multiple
outputs, latest results replace the earlier ones like explained in the previous
section. Also this merging strategy requires the top-level test suites to
be same in all outputs.

이것처럼 결과를 취합할 때, 결과 아웃풋은 아웃풋 파일에서 주어진 모든 것들을 갖는 모든 테스트와 스위트를 포함한다.
만약 복수의 결과에서 테스트를 찾는다면, 앞서 설명했듯이 최근 결과는 이전 것을 덮어쓰기한다.
또한 이 취합 전략은 모든 아웃풋에서 동일하게 top-level 테스트 스위트를 필요로한다.