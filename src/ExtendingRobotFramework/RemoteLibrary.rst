Remote library interface
========================

..
   The remote library interface provides means for having test libraries
   on different machines than where Robot Framework itself is running,
   and also for implementing libraries using other languages than the
   natively supported Python and Java. For a test library user remote
   libraries look pretty much the same as any other test library, and
   developing test libraries using the remote library interface is also
   very close to creating `normal test libraries`__.

원격 라이브러리 인터페이스는 Robot Framework가 실행되고 있는 다른
머신의 테스트 라이브러리를 가져오기 위한 도구를 제공한다. 또
기본적으로 지원하는 Python,java 외의 다른 언어를 사용하는 라이브러리를
구현하기 위한 도구를 제공한다. 테스트 라이브러리 사용자의 원격
라이브러리는 다른 테스트 라이브러리와 꽤 같은것으로 보인다. 그리고
원격 라이브러리 인터페이스를 사용하는 테스트 라이브러리를 개발하는
것은 `일반적인 테스트 라이브러리`__ 를 만드는 것과 매우 유사하다.


__ `Creating test libraries`_


.. contents::
   :depth: 2
   :local:

Introduction
------------

..
   There are two main reasons for using the remote library API:

원격 라이브러리 API를 쓰는 데에는 두가지 주요 이유가 있다:

..
   * It is possible to have actual libraries on different machines than
     where Robot Framework is running. This allows interesting
     possibilities for distributed testing.

   * Test libraries can be implemented using any language that supports
     `XML-RPC`_ protocol. At the time of this writing `there exists ready-made
     remote servers`__ for Python, Java, Ruby, .NET, Clojure, Perl and node.js.

* Robot Framework이 동작는 경우 다른 머신에서 실제 라이브러리를 가지고 있을 수도 있다.
  이것은 분산 시험을 위한 흥미로운 가능성 일수 있다.

* 테스트 라이브러리는 `XML-RPC`_ 프로토콜을 지원하는 다른 언어를 사용하여 구현될 수 있다.
  이 글을 쓰는 시점에서 Python, Java, Ruby, .NET, Clojure, Perl and node.js 위해 `이미 만들어진 원격 서버가 있다.`__

..
   The remote library interface is provided by the Remote library that is
   one of the `standard libraries`_.
   This library does not have any keywords of its own, but it works
   as a proxy between the core framework and keywords implemented
   elsewhere. The Remote library interacts with actual library
   implementations through remote servers, and the Remote library and
   servers communicate using a simple `remote protocol`_ on top of an
   XML-RPC channel.  The high level architecture of all this is
   illustrated in the picture below:

원격 라이브러리 인터페이스는 `표준 라이브러리 <standard libraries_>`__
의 하나인 원격 라이브러리에서 제공한다. 이 라이브러리는 자신의
키워드도 가지지 않지만, core framework와 구현된 키워드 사이에서
프록시로서 동작한다. 원격 라이브러리는 실제 라이브러리 구현과 XML-RPC
channel 최상위의 간단한 `원격 프로토콜 <remote protocol_>`__ 을
사용하여 통신하는 리모트 서버, 리모트 라이브러리, 서버를 통해
상호작용한다. 모든 상위 레벨의 구조는 아래 그림에 도시되어있다.:


.. figure:: src/ExtendingRobotFramework/remote.png

   Robot Framework architecture with Remote library

..
   .. note:: The remote client uses Python's standard xmlrpclib__ module. It does
	     not support custom XML-RPC extensions implemented by some XML-RPC
	     servers.

.. note:: 원격 클라이언트는 Python 표준  xmlrpclib__ 모듈을 쓴다.
          이것은 XML-RPC 서버에 의해서 구현된 사용자 정의 XML-RPC 확장을 지원하지 않는다.

__ https://code.google.com/p/robotframework/wiki/RemoteLibrary#Available_remote_servers
__ http://docs.python.org/2/library/xmlrpclib.html

Taking Remote library into use
------------------------------

Importing Remote library
~~~~~~~~~~~~~~~~~~~~~~~~

..
   The Remote library needs to know the address of the remote server but
   otherwise importing it and using keywords that it provides is no
   different to how other libraries are used. If you need to use the Remote
   library multiple times in a test suite, or just want to give it a more
   descriptive name, you can import it using the `WITH NAME syntax`_.

원격 라이브러리는 원격 서버의 주소를 알아야하지만 그렇지 않으면 그것을
가져오는 것과 그것이 제공하는 키워드를 쓰는 것은 다른 라이브러리를
사용하는 방법과 같다. 하나의 test suite에서 원격 라이브러리를 여러 번
사용해야하는 경우, 또는 좀 더 설명이 포함 된 이름을 주길 원하는 경우,
`이름 문법 <WITH NAME syntax_>`__ 을 사용하여 그것을 가져올 수 있다.**


.. sourcecode:: robotframework

   *** Settings ***
   Library    Remote    http://127.0.0.1:8270       WITH NAME    Example1
   Library    Remote    http://example.com:8080/    WITH NAME    Example2
   Library    Remote    http://10.0.0.2/example    1 minute    WITH NAME    Example3

..
   The URL used by the first example above is also the default address
   that the Remote library uses if no address is given. Similarly port
   `8270` is the port that remote servers are expected to use by default.
   (82 and 70 are the ASCII codes of letters `R` and `F`, respectively.)

위의 첫번째 예에서 사용된 URL은, 어떤 주소도 지정되지 않으면 원격
라이브러리가 사용하는 기본주소이다. 마찬가지로 포트 `8270` 는 원격
서버의 기본 포트 값이다. (82 , 70 은 각 각 아스키 코드로 문자 `R` ,
`F` 이다.)


..
   .. note:: When connecting to the local machine, it is recommended to use
	     address `127.0.0.1` instead of `localhost`. This avoids
	     address resolution that can be extremely slow `at least on Windows`__.
	     Prior to Robot Framework 2.8.4 the Remote library itself used the
	     potentially slow `localhost` by default.

.. note:: 로컬 머신에 접속할때, `localhost` 대신에 `127.0.0.1` 을 쓰는
          것을 권한다. 이것은 `적어도 윈도우`__ 에서 주소 확인이 매우
          늦어지는 것을 방지할 수 있다. Robot Framework 2.8.4 이전엔
          원격 라이브러리는 잠재적으로 느려질수 있는 기본값
          `localhost` 을 썼다.

..
   .. note:: Notice that if the URI contains no path after the server address,
	     `xmlrpclib module`__ used by the Remote library will use
	     `/RPC2` path by default. In practice using
	     `http://127.0.0.1:8270` is thus identical to using
	     `http://127.0.0.1:8270/RPC2`. Depending on the remote server
	     this may or may not be a problem. No extra path is appended if
	     the address has a path even if the path is just `/`. For
	     example, neither `http://127.0.0.1:8270/` nor
	     `http://127.0.0.1:8270/my/path` will be modified.

.. note:: 만약 URI가 서버 주소 이후에 다른 경로를 포함하지 않는다면,
          원격 라이브러리가 쓰는 `xmlrpclib module`__ 은 기본적으로
          `/RPC2` 경로를 쓸 것이다. 실제로 `http://127.0.0.1:8270` 을
          쓰는 것과 `http://127.0.0.1:8270/RPC2` 을 쓰는 것은 같다.
          원격 서버에 따라서 이것은 문제 일수도 있고 아닐수도 있다.
          만약 주소가 경로를 가지고 있다면 그 경로가 `/` 이더라도
          추가되는 여분의 경로는 없다. 예를 들어
          `http://127.0.0.1:8270/` 과 `http://127.0.0.1:8270/my/path`
          는 수정되지 않는다.

..
   The last example above shows how to give a custom timeout to the Remote library
   as an optional second argument. The timeout is used when initially connecting
   to the server and if a connection accidentally closes. Timeout can be
   given in Robot Framework `time format`_ like `60s` or `2 minutes 10 seconds`.

위의 마지막 예제는 선택적인 두번째 인자로 원격 라이브러리에 사용자
정의 제한 시간을 제공하는 방법을 보여준다. 서버와 연결이 실수로 닫아진
경우, 서버와 초기 연결할 때 제한 시간이 적용된다. 제한 시간은 Robot
Framework 에 `60s` or `2 minutes 10 seconds` 같은 `시간형식 <time
format_>`__ 으로 주어진다.

..
   The default timeout is typically several minutes, but it depends on
   the operating system and its configuration. Notice that setting
   a timeout that is shorter than keyword execution time will interrupt
   the keyword.

기본 시간 제한은 보통 몇분이다. 하지만 운영체제나 구성에 의해
결정된다. 키워드 수행 시간보다가 더 짧은 시간 제한을 설정하는 것은
키워드를 방해할 것이다.

..
   .. note:: Support for timeouts is a new feature in Robot Framework 2.8.6.
	     Timeouts do not work with Python/Jython 2.5 nor with IronPython.


.. note:: 시간제한 기능은 Robot Framework 2.8.6의 신규
          기능이다.Support for timeouts is a new feature in Robot
          Framework 2.8.6. 시간제한 기능은 Python/Jython 2.5 ,
          IronPython 에서 동작하지 않는다.


__ http://stackoverflow.com/questions/14504450/pythons-xmlrpc-extremely-slow-one-second-per-call
__ https://docs.python.org/2/library/xmlrpclib.html

Starting and stopping remote servers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Before the Remote library can be imported, the remote server providing
   the actual keywords must be started.  If the server is started before
   launching the test execution, it is possible to use the normal
   :setting:`Library` setting like in the above example. Alternatively other
   keywords, for example from Process_ or SSH__ libraries, can start
   the server up, but then you may need to use `Import Library keyword`__
   because the library is not available when the test execution starts.

원격 라이브러리를 가져오기 전에, 원격 서버가 제공하는 실제 키워드는
시작되어야 한다. 만약 서버가 시험 수행 전에 시작된다면, 위의 예처럼
정상 :setting:`Library` 설정은 사용될 수 있다. 대안적으로 다른
키워드는, 예를 들어 Process_ or SSH__ 라이브러리들은 서버를 시작시킬
수 있다. 하지만 테스트 실행을 시작할 때 라이브러리를 사용할 수 없기
때문에 `라이브러리 키워드를 가져와서`__ 을 사용해야 할 수도 있다.

..
   How a remote server can be stopped depends on how it is
   implemented. Typically servers support the following methods:

이것이 어떻게 구현되었는가 따라 원격 서버는 멈출 수 있다. 일반적으로
서버는 아래 방법을 지원한다. :

..
   * Regardless of the library used, remote servers should provide :name:`Stop
     Remote Server` keyword that can be easily used by executed tests.
   * Remote servers should have `stop_remote_server` method in their
     XML-RPC interface.
   * Hitting `Ctrl-C` on the console where the server is running should
     stop the server.
   * The server process can be terminated using tools provided by the
     operating system (e.g. ``kill``).

   .. note:: Servers may be configured so that users cannot stop it with
	     :name:`Stop Remote Server` keyword or `stop_remote_server`
	     method.


* 사용되는 라이브러리에 상관없이, 원격 서버는 :name:`Stop Remote
  Server` 키워드를 제공해야 한다. 이 키워드는 시험을 수행할때 쉽게
  사용 할수있다.
* 원격 서버는 XML-RPC 인터페이스에 `원격 서버를 멈추는` 방법을 가지고
  있어야한다.
* 서버가 동작하고 있는 콘솔에 `Ctrl-C` 을 입력하면, 서버가 멈춘다.
* 서버 프로세스는 운영 시스템이 제공하는 도구에 의해 종료 될
  수있다(e.g. ``kill``).

.. note:: 서버는 사용자가 :name:`Stop Remote Server` 키워드나
         `stop_remote_server` 방법을 사용하여 서버를 멈출 수 없도록
         구성할 수 있다.


__ https://github.com/robotframework/SSHLibrary
__ `Using Import Library keyword`_


Supported argument and return value types
-----------------------------------------

..
   Because the XML-RPC protocol does not support all possible object
   types, the values transferred between the Remote library and remote
   servers must be converted to compatible types. This applies to the
   keyword arguments the Remote library passes to remote servers and to
   the return values servers give back to the Remote library.

XML-RPC 프로토콜이 모든 가능한 객체형을 지원하지 않기 때문에, 원격
라이브러리와 원격 서버 사이에 전송되는 값은 호환가능한 유형으로
변환해야 한다. 이것은 원격 라이브러리가 원격 서버로 전달하는 키워드
인자와, 서버가 원격 라이브러리로 다시 제공하는 리턴 값에 적용된다.

..
   Both the Remote library and the Python remote server handle Python values
   according to the following rules. Other remote servers should behave similarly.

원격 라이브러리와 파이썬 원격 서버는 다음 규칙에 따라 파이썬 값을
처리한다. 다른 원격 서버는 유사하게 동작한다.

..
   * Strings, numbers and Boolean values are passed without modifications.
     
* 문자열, 숫자들, 논리값은 변경없이 전달된다.

..
   * Python `None` is converted to an empty string.
     
* 파이썬  `None` 은 공백으로 빈 문자열로 변경된다.

..
   * All lists, tuples, and other iterable objects (except strings and
     dictionaries) are passed as lists so that their contents are converted
     recursively.
     
* 모든 리스트, 튜블, 다른 반복가능한 객채들은(문자열과 딕셔너리를
  제외하고) 그 내용이 재귀적으로 반복되도록 리스트로 전달된다.

..
   * Dictionaries and other mappings are passed as dicts so that their keys are
     converted to strings and values converted to supported types recursively.
     
* 딕셔너리와 다른 맵핑들은 자신의 키를 반복적으로 지원하는 유형으로
  변한 문자열과 값으로 변환되도록 dicts로 전달된다.

..
   * Returned dictionaries are converted to so called *dot-accessible dicts*
     that allow accessing keys as attributes using the `extended variable syntax`_
     like `${result.key}`. This works also with nested dictionaries like
     `${root.child.leaf}`.

* 반환 딕셔너리를 소위 *dot-accessible dicts* 불리는 것으로 변경한다.
  이것은 `${result.key}` 같은 `extended variable syntax`_ 확장 변수를
  사용하여 속성으로서 키에 접근 할수있다. 이것은 `${root.child.leaf}`
  같은 중첩된 딕셔너리(nested dictionaries)로도 동작한다.

..
   * Strings containing bytes in the ASCII range that cannot be represented in
     XML (e.g. the null byte) are sent as `Binary objects`__ that internally use
     XML-RPC base64 data type. Received Binary objects are automatically converted
     to byte strings.

* XML로 (예를 들어, 널 바이트)를 표시 할 수없는 ASCII 범위의 바이트를
  포함하는 문자열은 내부적으로 XML-RPC Base64로 데이터 형식을 사용하는
  `이진 객체들로`__ 로 전송된다. 수신된 이진 객체는 자동적으로 바이트
  문자열로 변환된다.

..
   * Other types are converted to strings.
     
* 다른타입은 문자열로 변경된다.

..
   .. note:: Prior to Robot Framework 2.8.3, only lists, tuples, and dictionaries
	     were handled according to the above rules. General iterables and
	     mappings were not supported. Additionally binary support is new in
	     Robot Framework 2.8.4 and returning dot-accessible dictionaries new
	     in Robot Framework 2.9.

.. note:: Robot Framework 2.8.3 이전에는 단지 리스트, 튜플, 딕셔너리가
          위의 규칙을 따라 다뤄졌다. 일반적인 반복과 맵핑이 지원되지
          않았다. 추가적으로 바이너리 지원은 Robot Framework 2.8.4의
          새로운 기능이고 dot-accessible 딕셔너리는 Robot Framework
          2.9의 새로운 기능이다.

__ http://docs.python.org/2/library/xmlrpclib.html#binary-objects


Remote protocol
---------------

..
   This section explains the protocol that is used between the Remote
   library and remote servers. This information is mainly targeted for
   people who want to create new remote servers. The provided Python and
   Ruby servers can also be used as examples.

이번 절은 원격 라이브러리와 원격 서버 사이에 사용되는 프로토콜에 대해 설명한다.
이 정보는 주요하게 새로운 원격 서버를 만드려는 사람에게 필요하다.
이것은 Python과 Ruby 서버도 예처럼 사용할 수 있다.

..
   The remote protocol is implemented on top of `XML-RPC`_, which is a
   simple remote procedure call protocol using XML over HTTP. Most
   mainstream languages (Python, Java, C, Ruby, Perl, Javascript, PHP,
   ...) have a support for XML-RPC either built-in or as an extension.

원격 프로토콜은 HTTP를 통해 XML을 사용하여 간단한 원격 프로시저 호출
프로토콜 `XML-RPC`_ 맨 위에 구현된다. 가장 주요한 언어들(Python, Java,
C, Ruby, Perl, Javascript, PHP,...)은 built-in 이거나 extension로
XML-PRC를 지원한다.

Required methods
~~~~~~~~~~~~~~~~

..
   A remote server is an XML-RPC server that must have the same methods
   in its public interface as the `dynamic library API`_ has. Only
   `get_keyword_names` and `run_keyword` are actually
   required, but `get_keyword_arguments` and
   `get_keyword_documentation` are also recommended. Notice that
   using camelCase format in method names is not possible currently. How
   the actual keywords are implemented is not relevant for the Remote
   library.  A remote server can either act as a wrapper for real test
   libraries, like the provided Python and Ruby servers do, or it can
   implement keywords itself.

원격 서버는 `동적 라이브러리 API <dynamic library API_>`__ 가 가진
것처럼 공용 인터페이스 안에 동일한 method를 가지고 있는 XML-RPC
서버입니다. `get_keyword_names`와 `run_keyword` 만 실제로 필요하지만,
`get_keyword_arguments` 와 `get_keyword_documentation` 도 추천된다.
method 이름으로 camelCase를 쓰는 것은 현재 불가능하다. 어떻게 실제
키워드가 구현되었는 가는 원격 라이브러리와 관련없다. 원격 서버는
Python 과 Ruby 서버와 동일하게 실제 테스트를 위한 래퍼로 동작하거나
스스로 키워드 자체를 구현할 수 있다.

..
   Remote servers should additionally have `stop_remote_server`
   method in their public interface to ease stopping them. They should
   also automatically expose this method as :name:`Stop Remote Server`
   keyword to allow using it in the test data regardless of the test
   library. Allowing users to stop the server is not always desirable,
   and servers may support disabling this functionality somehow.
   The method, and also the exposed keyword, should return `True`
   or `False` depending was stopping allowed or not. That makes it
   possible for external tools to know did stopping the server succeed.

원격 서버는 멈춘것을 지우기 위해서 그들의 공용 인터페이스에 추가적으로
`stop_remote_server` method를 가져야 한다. 그들은 자동적으로 테스트
라이브러리에 상관없이 테스트 데이타를 쓸수 있도록 하기 위해,
:name:`Stop Remote Server` 키워드로 이 method를 드러낸다. 사용자가
서버를 멈출수 있는 권한을 주는 것이 항상 바람직한 것은 아니다. 그리고
서버는 어떻게든 이 기능을 사용하지 않도록 할수 있다. method과 노출된
키워드는 정지 혀용 여부에 따라 `True` 또는 `False` 을 반환해야한다.
이것은 서버를 멈추는것이 성공되었는지에 대해 외부툴이 알수있게 한다.

..
   The provided Python remote server can be used as a reference
   implementation.

제공된 Python 원격 서버는 참조 구현으로 쓰일수 있다.

Getting remote keyword names and other information
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   The Remote library gets a list of keywords that the remote server
   provides using `get_keyword_names` method. This method must
   return the keyword names as a list of strings.

원격 라이브러리는 리모트 서버가 `get_keyword_names` method 를 사용하여
제공할 수 있는 키워드의 리스트를 가져온다. 이 method는 키워드의 이름을
문자열 리스트로 반환해야 한다.

..
   Remote servers can, and should, also implement
   `get_keyword_arguments` and `get_keyword_documentation`
   methods to provide more information about the keywords. Both of these
   keywords get the name of the keyword as an argument. Arguments must be
   returned as a list of strings in the `same format as with dynamic
   libraries`__, and documentation must be returned `as a string`__.

원격 서버는 키워드에 대한 더 많은 정보를 제공하기 위해
`get_keyword_arguments` 와 `get_keyword_documentation` method 를
실행할 수있고 해야한다. 두 키워드 모두 인자로 부터 키워드 이름을
가져온다. 인자는 `동적 라이브러이와 같은 형식의`__ 문자열 리스트로
반환되어야 한다. 그리고 문서는 `문자열로`__ 반환되어야 한다.

..
   Remote servers can also provide `general library documentation`__ to
   be used when generating documentation with the Libdoc_ tool.

원격 서버는 Libdoc_ tool 로 문서를 생성할때 쓰기 위해 `일반적인
라이브러리 문서`__ 를 제공할 수 있다.

__ `Getting keyword arguments`_
__ `Getting keyword documentation`_
__ `Getting general library documentation`_


Executing remote keywords
~~~~~~~~~~~~~~~~~~~~~~~~~

..
   When the Remote library wants the server to execute some keyword, it
   calls remote server's `run_keyword` method and passes it the
   keyword name, a list of arguments, and possibly a dictionary of
   `free keyword arguments`__. Base types can be used as
   arguments directly, but more complex types are `converted to supported
   types`__.

원격 라이브러리 서버가 어떤 키워드를 실행하기를 원할 때, 그것은 원격
서버의 `run_keyword` method를 호출하고 그것을 가능하게 키워드 이름,
인자의 리스트 및 `free 키워드 인자`__ 의 딕셔너리를 전달한다. 기본
타입은 직접 인자로 사용될 수 있지만, 더 복잡한 유형은 `지원하는
타입으로 변환된다`__ .

..
   The server must return results of the execution in a result dictionary
   (or map, depending on terminology) containing items explained in the
   following table. Notice that only the `status` entry is mandatory,
   others can be omitted if they are not applicable.


서버가 아래 표에서 설명될 아이템을 포함한 결과 딕셔너리에(용어에 따라
map에) 실행의 결과를 반환해야 한다. `status` 항목이 필수 사항이고 다른
것들은 그들이 적용되지 않는 다면 생략 될수 있음을 알아야 한다.


..
   .. table:: Entries in the remote result dictionary
      :class: tabular

      +------------+-------------------------------------------------------------+
      |     Name   |                         Explanation                         |
      +============+=============================================================+
      | status     | Mandatory execution status. Either PASS or FAIL.            |
      +------------+-------------------------------------------------------------+
      | output     | Possible output to write into the log file. Must be given   |
      |            | as a single string but can contain multiple messages and    |
      |            | different `log levels`__ in format `*INFO* First            |
      |            | message\n*HTML* <b>2nd</b>\n*WARN* Another message`. It     |
      |            | is also possible to embed timestamps_ to the log messages   |
      |            | like `*INFO:1308435758660* Message with timestamp`.         |
      +------------+-------------------------------------------------------------+
      | return     | Possible return value. Must be one of the `supported        |
      |            | types`__.                                                   |
      +------------+-------------------------------------------------------------+
      | error      | Possible error message. Used only when the execution fails. |
      +------------+-------------------------------------------------------------+
      | traceback  | Possible stack trace to `write into the log file`__ using   |
      |            | DEBUG level when the execution fails.                       |
      +------------+-------------------------------------------------------------+
      | continuable| When set to `True`, or any value considered                 |
      |            | `True` in Python, the occurred failure is considered        |
      |            | continuable__. New in Robot Framework 2.8.4.                |
      +------------+-------------------------------------------------------------+
      | fatal      | Like `continuable`, but denotes that the occurred           |
      |            | failure is fatal__. Also new in Robot Framework 2.8.4.      |
      +------------+-------------------------------------------------------------+


.. table:: 원격 결과 딕셔너리의 목록
   :class: tabular

   +------------+-------------------------------------------------------------+
   |     Name   |                         Explanation                         |
   +============+=============================================================+
   | status     | 필수 실행 상태.  PASS 또는 FAIL.                            |
   +------------+-------------------------------------------------------------+
   | output     | 로그파일에 기입 가능한 출력.                                |
   |            | 한개의 스트링으로 주어져야하며, 여러개의 메세지와 다른      |
   |            | `log levels`__  을 *INFO* First message\n*HTML* <b>2        |
   |            | *WARN* Another message` 형식으로 포함할 수 있다.            |
   |            | `*INFO:1308435758660* Message with timestamp` 처럼 로그     |
   |            | 에 시간 기록을 넣는 것이 가능하다.                          |
   +------------+-------------------------------------------------------------+
   | return     | 값을 반환하는 것이 가능하다.                                |
   |            |   `지원하는 타입들`__  중에 하나여야 한다.                  |
   +------------+-------------------------------------------------------------+
   | error      | 가능한 에러 메세지, 실행이 실패일때만 쓰인다.               |
   +------------+-------------------------------------------------------------+
   | traceback  | 시험이 실패했을때, DEBUG 레벨을 사용하여                    |
   |            | `로그 파일에 기록 하는`__  스택 트레이스가 가능하다.        |
   +------------+-------------------------------------------------------------+
   | continuable| `True` 나 Python에서 `True` 로 간주되는 어떤값의 상태라면   |
   |            | 실패가 발생하면 continuable__  로 간주된다.                 |
   |            | Robot Framework 2.8.4 의 신규기능이다.                      |
   +------------+-------------------------------------------------------------+
   | fatal      | `continuable` 처럼,  치명적인__   실패의 발생에 대해서      |
   |            | 알린다. Robot Framework 2.8.4 의 신규기능이다.              |
   +------------+-------------------------------------------------------------+




__ `Different argument syntaxes`_
__ `Supported argument and return value types`_

__ `Logging information`_
__ `Supported argument and return value types`_
__ `Reporting keyword status`_
__ `Continue on failure`_
__ `Stopping test execution gracefully`_


Different argument syntaxes
~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   The Remote library is a `dynamic library`_, and in general it handles
   different argument syntaxes `according to the same rules`__ as any other
   dynamic library.
   This includes mandatory arguments, default values, varargs, as well
   as `named argument syntax`__.


원격 라이브러리는 `동적 라이브러리 <dynamic library_>`__ 이며,
일반적으로는 다른 동적 라이브러리와 `같은 규칙에 따라`__ 다른 인자
구문을 처리한다. 이것은 필수 인자, 기본값, 가변 인자뿐만 아니라
`이름이 붙여진 인자구문`__ 을 포함한다.

..
   Also free keyword arguments (`**kwargs`) works mostly the `same way
   as with other dynamic libraries`__. First of all, the
   `get_keyword_arguments` must return an argument specification that
   contains `**kwargs` exactly like with any other dynamic library.
   The main difference is that remote servers `run_keyword` method must have optional third argument
   that gets the kwargs specified by the user. The third argument must be optional
   because, for backwards-compatibility reasons, the Remote library passes kwargs
   to the `run_keyword` method only when they have been used in the test data.

또한 free 키워드 인자 (`** kwargs`)는 대부분 `다른 동적 라이브러리와
같은 방식으로`__ 동작한다.. 우선, `get_keyword_arguments` 는 다른 동적
라이브러리와 정확하게 같은 `**kwargs`를 포함하는 인자 명세서를
반환해야한다. 가장 큰 차이점은 원격 서버의 `run_keyword` 키워드 실행
방법이 사용자가 지정한 kwargs로 얻은 선택적인 세번째 인수를 가지고
있어야 한다는 것이다. 세번째 인자는 선택적인 것이다. 역방향 호환성을
위해, 그들이 테스트 데이터에 사용된 경우에, 원격 라이브러리들이
kwargs를 `run_keyword` 방법에 전달한다.

..
   In practice `run_keyword` should look something like the following
   Python and Java examples, depending on how the language handles optional
   arguments.


실제로 `run_keyword` 는 언어가 선택적인 인자를 어떻게 사용하는지에
따라 다음 Python ,Java 예처럼 보인다.

.. sourcecode:: python

    def run_keyword(name, args, kwargs=None):
        # ...




.. sourcecode:: java
    public Map run_keyword(String name, List args) {
        // ...
    }

    public Map run_keyword(String name, List args, Map kwargs) {
        // ...
    }

..
   .. note:: Remote library supports `**kwargs` starting from
	     Robot Framework 2.8.3.

.. note:: 원격 라이브러리는 Robot Framework 2.8.3 부터 `**kwargs` 를
          지원한다.
	  
__ `Getting keyword arguments`_
__ `Named argument syntax with dynamic libraries`_

__ `Free keyword arguments with dynamic libraries`_



























































