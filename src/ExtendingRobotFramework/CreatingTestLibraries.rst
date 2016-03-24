Creating test libraries
=======================

..
   Robot Framework's actual testing capabilities are provided by test
   libraries. There are many existing libraries, some of which are even
   bundled with the core framework, but there is still often a need to
   create new ones. This task is not too complicated because, as this
   chapter illustrates, Robot Framework's library API is simple
   and straightforward.

Robot Framework의 실제 테스팅 기능은 테스트 라이브러리에 의해
제공된다. 코어 라이브러리와 함께 제공되는 많은 라이브러리가 있다.
하지만 여전히 종종 새로 생성할 필요가 있다. 이 장에서는 Robot
Framework의 라이브러리 API가 단순하고 간단하기 때문에 이 작업이 그리
복잡하지는 않다.

.. contents::
   :depth: 2
   :local:

Introduction
------------

Supported programming languages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Robot Framework itself is written with Python_ and naturally test
   libraries extending it can be implemented using the same
   language. When running the framework on Jython_, libraries can also be
   implemented using Java_. Pure Python code works both on Python and
   Jython, assuming that it does not use syntax or modules that are not
   available on Jython. When using Python, it is also possible to
   implement libraries with C using `Python C API`__, although it is
   often easier to interact with C code from Python libraries using
   ctypes__ module.

Robot Framework 자체는 Python_ 으로 작성되었고 자연스럽게 테스트
라이브러리 확장은 동일 언어를 사용하여 구현될 수 있다. Jython_ 에서
프레임워크가 실행되면 또한 라이브러리는 Java_ 를 사용하여 구현 될 수
있다. 순수한 파이썬 코드는 Jython에서만 사용가능한 문법이나 모듈을
사용하지 않는한 파이썬과 Jython에서 동작한다. 파이썬을 사용할 때 비록
ctypes__ 모듈을 사용하여 파이썬 라이브러리로 부터 C 코드와 상호
작용하는 것이 더 쉽지만, `Python C API`__ 를 사용하여 C와 함께
라이브러리를 구현 할 수 있다.

..
   Libraries implemented using these natively supported languages can
   also act as wrappers to functionality implemented using other
   programming languages. A good example of this approach is the `Remote
   library`_, and another widely used approaches is running external
   scripts or tools as separate processes.

본래 지원된 언어를 사용하여 구현된 라이브러리는 다른 프로그래밍 언어를
사용하여 구현된 기능에 대한 래퍼(wrappers) 역할을 할 수 있다. 이런
접근에 대한 좋은 예제는 `Remote library`_ 이다. 널리 사용되는 다른
접근은 별도의 프로세스로 외부 스크립트나 툴을 수행할 수 있다.

..
   .. tip:: `Python Tutorial for Robot Framework Test Library Developers`__
	    covers enough of Python language to get started writing test
	    libraries using it. It also contains a simple example library
	    and test cases that you can execute and otherwise investigate
	    on your machine.

.. tip:: `Robot Framework 테스트 라이브러리 개발자를 위한 파이썬
         튜토리얼`__ 은 파이썬 언어로 테스트 라이브러리 작성을
         시작하는 사람에게 충분한 정보를 제공한다. 또한 간단한 예제
         라이브러리와 실행할 수 있는 test cases 및 그 밖의 연구해야 할
         다양한 주제를 포함한다.

__ http://docs.python.org/c-api/index.html
__ http://docs.python.org/library/ctypes.html
__ http://code.google.com/p/robotframework/wiki/PythonTutorial

Different test library APIs
~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Robot Framework has three different test library APIs.

Robot Framework는 3가지 다른 테스트 라이브러리 API를 가진다.
   
Static API

  가장 간단한 방법은 `keyword names`_ 에 직접 매핑되는
  메소드를 가지는 모듈(파이썬) 또는 클래스(파이썬 또는 자바)를 가지는
  것이다. 키워드는 메소드가 구현된 것과 같이 동일한
  `전달인자(arguments)`__ 를 가진다. 예외처리로 `실패를 리포트`__
  하고, 표준 출력에 `로그`__ 를 적는 키워드는 `반환` 문을 사용하여 `값을 반환`__ 할 수 있다.


     
Dynamic API

  동적 라이브러리는 클래스다. 클래스는 구현한 키워드의 이름을 얻는
  메소드를 구현하고, 다른 메소드는 주어진 전달인자로 명명된 키워드를
  수행한다. 구현하기 위한 키워드의 이름 뿐만아니라 실행되는 방법은
  런타임에 동적으로 결정될 수 있지만 상태보고, 로깅, 리턴 값은 정적
  API와 비슷하게 행해진다.

..
     Dynamic libraries are classes that implement a method to get the names
     of the keywords they implement, and another method to execute a named
     keyword with given arguments. The names of the keywords to implement, as
     well as how they are executed, can be determined dynamically at
     runtime, but reporting the status, logging and returning values is done
     similarly as in the static API.

Hybrid API

  이것은 정적과 동적 API간의 하이브리드다. 라이브러리는 구현할
  키워드를 말해주는 메소드를 가지는 클래스다. 하지만 이런 키워드는
  반드시 직접 사용가능해야 한다. 어떤 키워드가 구현되는 지를 밝히는
  것을 제외한 모든 것은 정적 API에서와 유사하다.

..
     This is a hybrid between the static and the dynamic API. Libraries are
     classes with a method telling what keywords they implement, but
     those keywords must be available directly. Everything else except
     discovering what keywords are implemented is similar as in the
     static API.

..
   All these APIs are described in this chapter. Everything is based on
   how the static API works, so its functions are discussed first. How
   the `dynamic library API`_ and the `hybrid library API`_ differ from it
   is then discussed in sections of their own.

이 모든 API는 이 장에서 설명한다. 모든 것은 정적 API가 작동하는 방법을
기반이다. 그래서 그 기능이 먼저 논의된다. `dynamic library API`_ 와
`hybrid library API`_ 가 어떻게 다른지는 각각의 절에서 설명한다.
   
..
   The examples in this chapter are mainly about using Python, but they
   should be easy to understand also for Java-only developers. In those
   few cases where APIs have differences, both usages are explained with
   adequate examples.

이 장의 예제는 주로 파이썬을 사용하지만 자바 전용 개발자 또한 이해하기
쉽다. API 차이가 있는 경우 경우 두 사용법 모두 적절한 예제와 같이
설명한다.
   
__ `Keyword arguments`_
__ `Reporting keyword status`_
__ `Logging information`_
__ `Returning values`_

Creating test library class or module
-------------------------------------

..
   Test libraries can be implemented as Python modules and Python or Java
   classes.

테스트 라이브러리는 파이썬 모듈 및 파이썬 또는 자바 클래스로 구현할 수 있다.

Test library names
~~~~~~~~~~~~~~~~~~

..
   The name of a test library that is used when a library is imported is
   the same as the name of the module or class implementing it. For
   example, if you have a Python module `MyLibrary` (that is,
   file :file:`MyLibrary.py`), it will create a library with name
   :name:`MyLibrary`. Similarly, a Java class `YourLibrary`, when
   it is not in any package, creates a library with exactly that name.

라이브러리가 임포트될때 사용되는 이름은 모듈 또는 클래스의 이름과
같다. 예를 들어 파이썬 모듈 `MyLibrary` (즉 :file:`MyLibrary.py`
파일)는 :name:`MyLibrary` 이름을 가지는 라이브러리를 만든다. 비슷하게
어떤 패키지에도 없는 `YourLibrary` 자바 클래스는 같은 이름의
라이브러리를 만든다.

..
   Python classes are always inside a module. If the name of a class
   implementing a library is the same as the name of the module, Robot
   Framework allows dropping the class name when importing the
   library. For example, class `MyLib` in :file:`MyLib.py`
   file can be used as a library with just name :name:`MyLib`. This also
   works with submodules so that if, for example, `parent.MyLib` module
   has class `MyLib`, importing it using just :name:`parent.MyLib`
   works. If the module name and class name are different, libraries must be
   taken into use using both module and class names, such as
   :name:`mymodule.MyLibrary` or :name:`parent.submodule.MyLib`.

파이썬 클래스는 항상 모듈 내부에 존재한다. 라이브러리를 구현하는
클래스 이름이 모듈의 이름과 같다면 Robot Framework는 라이브러리를
임포트 할때 클래스 이름을 생략할 수 있다. 예를 들면, :file:`MyLib.py`
파일의 클래스 `MyLib` 는 단지 :name:`MyLib` 이름만으로 라이브러리로
사용될 수 있다. 또한 서브 모듈과 함께 동작한다. 예를 들어
`parent.MyLib` 모듈이 `MyLib` 클래스를 가질 경우 단지
:name:`parent.MyLib` 만을 사용하여 임포트할 수 있다. 모듈 이름과
클래스 이름이 다른 경우 라이브러리는 :name:`mymodule.MyLibrary` 나
:name:`parent.submodule.MyLib` 처럼 반드시 모듈과 클래스 이름을
사용해야 한다.
      
..
   Java classes in a non-default package must be taken into use with the
   full name. For example, class `MyLib` in `com.mycompany.myproject`
   package must be imported with name :name:`com.mycompany.myproject.MyLib`.

기본이 아닌 패키지의 자바 클래스는 반드시 전체 이름으로 사용해야 한다.
예를 들어 `com.mycompany.myproject` 패키지의 `MyLib` 클래스는
:name:`com.mycompany.myproject.MyLib` 이름으로 임포트해야 한다.
   
..
   .. note:: Dropping class names with submodules works only in Robot Framework
	     2.8.4 and newer. With earlier versions you need to include also
	     the class name like :name:`parent.MyLib.MyLib`.

.. note:: 서브 모듈과 클래스 이름을 생략하는 것은 Robot Framework
          2.8.4부터 지원한다. 이전 버전에서는
          :name:`parent.MyLib.MyLib` 같이 클래스 이름을 써야 한다.
	  
..
   .. tip:: If the library name is really long, for example when the Java
	    package name is long, it is recommended to give the library a
	    simpler alias by using the `WITH NAME syntax`_.

.. tip:: 자바 패키지 이름이 긴 경우 처럼 라이브러리 이름이 진짜 긴
         경우, 예를 들어 `WITH NAME syntax`_ 를 사용하여 간단한 별명을
         사용할 것을 권장한다.
	    
Providing arguments to test libraries
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   All test libraries implemented as classes can take arguments. These
   arguments are specified in the Setting table after the library name,
   and when Robot Framework creates an instance of the imported library,
   it passes them to its constructor. Libraries implemented as a module
   cannot take any arguments, so trying to use those results in an error.

클래스로 구현된 모든 테스트 라이브러리는 전달인자를 취할 수 있다. 이
인수는 Setting 표에서 라이브러리 이름 뒤에 지정할 수 있고, Robot
Framework이 임포트된 라이브러리의 인스턴스를 만들때 생성자에 전달한다.
모듈로 구현된 라이브러리는 인수를 취할 수 없기 때문에 에러를 반환한다.

..
   The number of arguments needed by the library is the same
   as the number of arguments accepted by the library's
   constructor. The default values and variable number of arguments work
   similarly as with `keyword arguments`_, with the exception that there
   is no variable argument support for Java libraries. Arguments passed
   to the library, as well as the library name itself, can be specified
   using variables, so it is possible to alter them, for example, from the
   command line.

라이브러리에서 필요로 하는 전달인자의 갯수는 라이브러리의 생성자가
받을 수 있는 인자의 수와 동일하다. 기본 값과 가변인자는 자바
라이브러리에 대한 가변 인자 지원이 없다는 것을 제외하고, `keyword
arguments`_ 와 비슷하게 작동한다. 라이브러리에 전달되는 전달인자뿐만
아니라 라이브러리 이름 자체는 변수를 사용하여 지정할 수 있고
명령행에서 변경할 수 있다.
   
.. sourcecode:: robotframework

   *** Settings ***
   Library    MyLibrary     10.0.0.1    8080
   Library    AnotherLib    ${VAR}

..
   Example implementations, first one in Python and second in Java, for
   the libraries used in the above example:

위의 예에서 사용되는 라이브러리를 처음에는 파이썬으로 두번째는 자바로
구현:
   
.. sourcecode:: python

  from example import Connection

  class MyLibrary:

      def __init__(self, host, port=80):
          self._conn = Connection(host, int(port))

      def send_message(self, message):
          self._conn.send(message)

.. sourcecode:: java

   public class AnotherLib {

       private String setting = null;

       public AnotherLib(String setting) {
           setting = setting;
       }

       public void doSomething() {
           if setting.equals("42") {
               // do something ...
           }
       }
   }

Test library scope
~~~~~~~~~~~~~~~~~~

..
   Libraries implemented as classes can have an internal state, which can
   be altered by keywords and with arguments to the constructor of the
   library. Because the state can affect how keywords actually behave, it
   is important to make sure that changes in one test case do not
   accidentally affect other test cases. These kind of dependencies may
   create hard-to-debug problems, for example, when new test cases are
   added and they use the library inconsistently.

클래스로 구현된 라이브러리는 내부 상태를 가진다. 라이브러리 생성자에
키워드와 전달인자로 변경할 수 있다. 상태는 어떻게 키워드가 실제
행동하는지에 영향을 미칠 수 있기 때문에 하나의 테스트 케이스의 변화가
우연히 다른 테스트 케이스에 영향을 주지 않도록 하는 것이 중요하다.
이런 종류의 의존성은 디버깅하기 어려운 문제를 생성할 수 있다. 예를
들어 새로운 테스트 케이스가 추가될 때 라이브러리 일관성을 유지해야
한다.
   
..
   Robot Framework attempts to keep test cases independent from each
   other: by default, it creates new instances of test libraries for
   every test case. However, this behavior is not always desirable,
   because sometimes test cases should be able to share a common
   state. Additionally, all libraries do not have a state and creating
   new instances of them is simply not needed.

Robot Framework은 서로 독립적인 test cases를 유지하려고 시도한다:
기본적으로 모든 test case에 대한 테스트 라이브러리의 새로운 인스턴스를
생성한다. 그러나 가끔 test cases는 공통의 상태를 공유할 수도 있기
때문에 이런 행동이 항상 바람직하지는 않다. 추가적으로 모든
라이브러리는 상태를 가지지 않고, 라이브러리의 새로운 인스턴스 생성은
단순히 필요하지 않다.
   
..
   Test libraries can control when new libraries are created with a
   class attribute `ROBOT_LIBRARY_SCOPE` . This attribute must be
   a string and it can have the following three values:

테스트 라이브러리는 새로운 라이브러리가 생성될 때 클래스 속성
`ROBOT_LIBRARY_SCOPE` 로 제어 할 수 있다. 이 속성은 문자열로 아래
3 가지 값을 가질 수 있다:

`TEST CASE`
  새로운 인스턴스는 모든 test case 마다 생성된다. Suite setup과 suite
  teardown은 아직 다른 인스턴스를 공유한다. 이 값이 기본값이다.

..
     A new instance is created for every test case. A possible suite setup
     and suite teardown share yet another instance. This is the default.

`TEST SUITE`
  새로운 인스턴스는 test suite마다 생성된다. Test cases를 포함하는
  test case 파일로부터 생성된 가장 저수준(lowest-level)의 test
  suites은 본인 소유의 인스턴스를 가지고, 고수준(higer-level)의
  suites은 자신의 setups과 teardowns을 위한 인스턴트를 얻는다.

..
     A new instance is created for every test suite. The lowest-level
     test suites, created from test case files and containing test cases,
     have instances of their own, and higher-level suites all get their
     own instances for their possible setups and teardowns.

`GLOBAL`
  전체 테스트 실행 중에 유일한 하나의 인스턴스가 생성되고, 모든 test
  cases와 test suites에 공유 된다. 모듈로 부터 생성된 라이브러리는
  항상 전역(global)이다.

..
     Only one instance is created during the whole test execution and it
     is shared by all test cases and test suites. Libraries created from
     modules are always global.

..
   .. note:: If a library is imported multiple times with different arguments__,
	     a new instance is created every time regardless the scope.

.. note:: 라이브러리가 다른 전달인자__ 로 여러번 임포트되면 범위에
          관계없이 매번 새로운 인스턴스가 생성된다.
	  
..
   When the `TEST SUITE` or `GLOBAL` scopes are used with test
   libraries that have a state, it is recommended that libraries have some
   special keyword for cleaning up the state. This keyword can then be
   used, for example, in a suite setup or teardown to ensure that test
   cases in the next test suites can start from a known state. For example,
   :name:`SeleniumLibrary` uses the `GLOBAL` scope to enable
   using the same browser in different test cases without having to
   reopen it, and it also has the :name:`Close All Browsers` keyword for
   easily closing all opened browsers.

`TEST SUITE` 또는 `GLOBAL` 범위가 상태를 가지는 테스트 라이브러리와
사용될 경우, 라이브러리는 상태를 깨끗이 하기 위한 약간 특별한 키워드를
가질 것을 권장한다. 이 키워드는 suite setup과 teardown에서 다음 test
suites의 test cases가 알려진 상태에서 시작할 수 있도록 사용 될 수
있다. 예를 들어, :name:`SeleniumLibrary` 는 다시 열지 않고 다른 test
cases에서도 동일한 브라우저를 사용하기 위해서 `GLOBAL` 범위를
사용한다. 또한 쉽게 열려있는 모든 브라우저를 닫기 위해서 :name:`Close
All Browsers` 키워드를 가진다.
   
..
   Example Python library using the `TEST SUITE` scope:

`TEST SUITE` 범위를 사용한 파이썬 라이브러리 예제:
   
.. sourcecode:: python

    class ExampleLibrary:

        ROBOT_LIBRARY_SCOPE = 'TEST SUITE'

        def __init__(self):
            self._counter = 0

        def count(self):
            self._counter += 1
            print self._counter

        def clear_counter(self):
            self._counter = 0

..
   Example Java library using the `GLOBAL` scope:

`GLOBAL` 범위를 사용하는 자바 라이브러리 예제:

.. sourcecode:: java

    public class ExampleLibrary {

        public static final String ROBOT_LIBRARY_SCOPE = "GLOBAL";

        private int counter = 0;

        public void count() {
            counter += 1;
            System.out.println(counter);
        }

        public void clearCounter() {
            counter = 0;
        }
    }

__ `Providing arguments to test libraries`_

Specifying library version
~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   When a test library is taken into use, Robot Framework tries to
   determine its version. This information is then written into the syslog_
   to provide debugging information. Library documentation tool
   Libdoc_ also writes this information into the keyword
   documentations it generates.

테스트 라이브러리가 사용하기 위해서 선택될 때 Robot Framework는 그것의
버전을 결정하려고 시도한다. 이 정보는 디버깅 정보를 제공하기 위해
syslog_ 에 기록된다. 또한, 라이브러리 문서화 툴 Libdoc_ 이런 정보를
키워드 문서에 기록한다.
   
..
   Version information is read from attribute
   `ROBOT_LIBRARY_VERSION`, similarly as `test library scope`_ is
   read from `ROBOT_LIBRARY_SCOPE`. If
   `ROBOT_LIBRARY_VERSION` does not exist, information is tried to
   be read from `__version__` attribute. These attributes must be
   class or module attributes, depending whether the library is
   implemented as a class or a module.  For Java libraries the version
   attribute must be declared as `static final`.

버전 정보는 `ROBOT_LIBRARY_VERSION` 속성에서 읽을 수 있다. 비슷하게
`test library scope`_ 는 `ROBOT_LIBRARY_SCOPE` 로 부터 읽는다.
`ROBOT_LIBRARY_VERSION` 이 없다면 정보는 `__version__` 속성으로 부터
읽으려고 시도한다. 이런 속성들은 반드시 클래스나 모듈의 속성이어야
하고, 라이브러리가 클래스나 모듈로 구현되었는지에 의존한다. 자바
라이브러리에서 버전 정보는 `static final` 로 선언해야 한다.

..
   An example Python module using `__version__`:

`__version__` 을 사용하는 파이썬 모듈 예제:
   
.. sourcecode:: python

    __version__ = '0.1'

    def keyword():
        pass

..
   A Java class using `ROBOT_LIBRARY_VERSION`:

`ROBOT_LIBRARY_VERSION` 을 사용하는 자바 클래스 :
   
.. sourcecode:: java

    public class VersionExample {

        public static final String ROBOT_LIBRARY_VERSION = "1.0.2";

        public void keyword() {
        }
    }

Specifying documentation format
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Starting from Robot Framework 2.7.5, library documentation tool Libdoc_
   supports documentation in multiple formats. If you want to use something
   else than Robot Framework's own `documentation formatting`_, you can specify
   the format in the source code using  `ROBOT_LIBRARY_DOC_FORMAT` attribute
   similarly as scope__ and version__ are set with their own
   `ROBOT_LIBRARY_*` attributes.

Robot Framework 2.7.5 부터 라이브러리 문서화 툴 Libdoc_ 은 여러 형식의
문서를 지원한다. Robot Framework의 `문서 형식 <documentation
formatting_>`__ 보다 다른 무언가를 사용하려면
`ROBOT_LIBRARY_DOC_FORMAT` 속성을 사용하여 소스 코드에 형식을 지정할
수 있다. 마찬가지로 범위__ 와 버전__ 은 `ROBOT_LIBRARY_*` 속성으로
설정할 수 있다.
   
..
   The possible case-insensitive values for documentation format are
   `ROBOT` (default), `HTML`, `TEXT` (plain text),
   and `reST` (reStructuredText_). Using the `reST` format requires
   the docutils_ module to be installed when documentation is generated.

문서 형식을 위한 가능한 값(대소문자구분)은 `ROBOT` (기본), `HTML`,
`TEXT` (일반 텍스트), `reST` (reStructuredText_) 이다. `reST` 형식을
사용하려면 문서를 생성하기 위하여 docutils_ 모듈을 먼저 설치해야 하다.

..
   Setting the documentation format is illustrated by the following Python and
   Java examples that use reStructuredText and HTML formats, respectively.
   See `Documenting libraries`_ section and Libdoc_ chapter for more information
   about documenting test libraries in general.

다음 예제는 파이썬과 자바로 reStructuredText 와 HTML 형식으로 문서
형식을 설정하는 것을 보여준다. 테스트 라이브러리 문서화에 대한 더
상세한 정보는 `Documenting libraries`_ 절과 Libdoc_ 장을 참고하라.
   
.. sourcecode:: python

    """A library for *documentation format* demonstration purposes.

    This documentation is created using reStructuredText__. Here is a link
    to the only \`Keyword\`.

    __ http://docutils.sourceforge.net
    """

    ROBOT_LIBRARY_DOC_FORMAT = 'reST'

    def keyword():
        """**Nothing** to see here. Not even in the table below.

        =======  =====  =====
        Table    here   has
        nothing  to     see.
        =======  =====  =====
        """
        pass

.. sourcecode:: java

    /**
     * A library for <i>documentation format</i> demonstration purposes.
     *
     * This documentation is created using <a href="http://www.w3.org/html">HTML</a>.
     * Here is a link to the only `Keyword`.
     */
    public class DocFormatExample {

        public static final String ROBOT_LIBRARY_DOC_FORMAT = "HTML";

        /**<b>Nothing</b> to see here. Not even in the table below.
         *
         * <table>
         * <tr><td>Table</td><td>here</td><td>has</td></tr>
         * <tr><td>nothing</td><td>to</td><td>see.</td></tr>
         * </table>
         */
        public void keyword() {
        }
    }

__ `Test library scope`_
__ `Specifying library version`_


Library acting as listener
~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   `Listener interface`_ allows external listeners to get notifications about
   test execution. They are called, for example, when suites, tests, and keywords
   start and end. Sometimes getting such notifications is also useful for test
   libraries, and they can register a custom listener by using
   `ROBOT_LIBRARY_LISTENER` attribute. The value of this attribute
   should be an instance of the listener to use, possibly the library itself.
   For more information and examples see `Test libraries as listeners`_ section.

`Listener interface`_ 는 외부 리스너가 테스트 수행에 대한 알림을 받을
수 있도록 한다. 예를 들어 suites, tests, keywords가 시작하고, 끝날 때
호출된다. 때로는 이러한 알림을 받는 것은 테스트 라이브러리에 대하여
유용하기 때문에 `ROBOT_LIBRARY_LISTENER` 속성을 사용하여 사용자 정의
리스너를 등록할 수 있다. 이 속성의 값은 가능한 라이브러리 그 자체를
사용하기 위한 리스너의 인스턴스이어야 한다. 더 자세한 정보와 예제는
`Test libraries as listeners`_ 절을 참고하라.
   
Creating static keywords
------------------------

What methods are considered keywords
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   When the static library API is used, Robot Framework uses reflection
   to find out what public methods the library class or module
   implements. It will exclude all methods starting with an underscore,
   and with Java libraries also methods that are implemented only in
   `java.lang.Object` are ignored. All the methods that are not
   ignored are considered keywords. For example, the Python and Java
   libraries below implement single keyword :name:`My Keyword`.

정적 라이브러리 API를 사용하는 경우, Robot Framework는 라이브러리
클래스나 모듈이 구현한 어떤 public 메소드를 찾기 위하여
반영(reflection) [#]_ 을 사용한다. 언더스코어(_)로 시작하는 모든
메소드를 제외하고, 자바 라이브러리에서 단지 `java.lang.Object` 안에
구현된 메소드들은 무시된다. 무시되지 않은 모든 메소드는 키워드로
간주된다. 예를 들어 아래 예제에서 파이썬과 자바 라이브러리는 하나의
키워드 :name:`My Keyword` 를 구현한다.

.. [#] `<위키 피디아 | 반영 <https://ko.wikipedia.org/wiki/%EB%B0%98%EC%98%81_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99)>`_
       
.. sourcecode:: python

    class MyLibrary:

        def my_keyword(self, arg):
            return self._helper_method(arg)

        def _helper_method(self, arg):
            return arg.upper()

.. sourcecode:: java

    public class MyLibrary {

        public String myKeyword(String arg) {
            return helperMethod(arg);
        }

        private String helperMethod(String arg) {
            return arg.toUpperCase();
        }
    }

..
   When the library is implemented as a Python module, it is also
   possible to limit what methods are keywords by using Python's
   `__all__` attribute. If `__all__` is used, only methods
   listed in it can be keywords. For example, the library below
   implements keywords :name:`Example Keyword` and :name:`Second
   Example`. Without `__all__`, it would implement also keywords
   :name:`Not Exposed As Keyword` and :name:`Current Thread`. The most
   important usage for `__all__` is making sure imported helper
   methods, such as `current_thread` in the example below, are not
   accidentally exposed as keywords.

라이브러리가 파이썬 모듈로 구현 될 때, 파이썬의 `__all__` 속성을
사용하여 키워드가 되는 메소드를 제한할 수 있다. `__all__` 의 목록에
있는 메소드만 키워드가 된다. 예를 들어 아래 예제 라이브러리는 키워드
:name:`Example Keyword` 와 :name:`Second Example` 을 구현한다.
`__all__` 이 없다면, 키워드 :name:`Not Exposed As Keyword` 와
:name:`Current Thread` 또한 구현한다. `__all__` 에 대한 가장 중요한
사용법은 임포트된 헬퍼 메소드를 확인하는 것이다. 아래 예제에서
`current thread` 는 실수로라도 키워드로 노출되지 않는다.

.. sourcecode:: python

   from threading import current_thread

   __all__ = ['example_keyword', 'second_example']

   def example_keyword():
       if current_thread().name == 'MainThread':
           print 'Running in main thread'

   def second_example():
       pass

   def not_exposed_as_keyword():
       pass

Keyword names
~~~~~~~~~~~~~

..
   Keyword names used in the test data are compared with method names to
   find the method implementing these keywords. Name comparison is
   case-insensitive, and also spaces and underscores are ignored. For
   example, the method `hello` maps to the keyword name
   :name:`Hello`, :name:`hello` or even :name:`h e l l o`. Similarly both the
   `do_nothing` and `doNothing` methods can be used as the
   :name:`Do Nothing` keyword in the test data.

테스트 데이타에 사용된 키워드 이름은 이러한 키워드를 구현하는 방법을
찾기 위해 메소드 이름과 비교된다. 이름 비교는 대소문자를 구분하고
공백과 언더스코아는 무시한다. 예를 들어 메소드 `hello` 는 키워드 이름
:name:`Hello`, :name:`hello` 심지어 :name:`h e l l o` 에 매핑된다.
비슷하게 `do_nothing` 과 `doNothing` 두 메소드는 테스트 데이타에서
:name:`Do Nothing` 키워드로 사용할 수 있다.
	 
..
   Example Python library implemented as a module in the :file:`MyLibrary.py` file:

:file:`MyLibrary.py` 파일에 모듈로 파이썬 라이브러리를 구현한 예:
   
.. sourcecode:: python

  def hello(name):
      print "Hello, %s!" % name

  def do_nothing():
      pass

..
   Example Java library implemented as a class in the :file:`MyLibrary.java` file:

:file:`MyLibrary.java` 파일에 클래스로 자바 라이브러리를 구현한 예:
   
.. sourcecode:: java

  public class MyLibrary {

      public void hello(String name) {
          System.out.println("Hello, " + name + "!");
      }

      public void doNothing() {
      }

  }

..
   The example below illustrates how the example libraries above can be
   used. If you want to try this yourself, make sure that the library is
   in the `module search path`_.

아래 예제는 위의 예제 라이브러리를 사용하는 방법을 보여준다. 이것이
동작하기 위해서 `모듈 검색 경로 <module search path_>`__ 안에
라이브러리가 있도록 해야 한다.
   
.. sourcecode:: robotframework

   *** Settings ***
   Library    MyLibrary

   *** Test Cases ***
   My Test
       Do Nothing
       Hello    world

Using a custom keyword name
'''''''''''''''''''''''''''

..
   It is possible to expose a different name for a keyword instead of the
   default keyword name which maps to the method name.  This can be accomplished
   by setting the `robot_name` attribute on the method to the desired custom name.
   The decorator `robot.api.deco.keyword` may be used as a shortcut for setting
   this attribute when used as follows:

메소드 이름에 매핑되는 기본 키워드 이름 대신에 다른 이름을 노출할 수
있다. 이것은 원하는 사용자 정의 이름에 `robot_name` 속성을
설정함으로써 달성 할 수 있다. 장식자(decorator)
`robot.api.deco.keyword` 는 다음과 같이 사용하는 경우에, 이 속성을
설정하는 바로가기(shortcut)로 사용할 수 있다.
   
.. sourcecode:: python

  from robot.api.deco import keyword

  @keyword('Login Via User Panel')
  def login(username, password):
      # ...

.. sourcecode:: robotframework

   *** Test Cases ***
   My Test
       Login Via User Panel    ${username}    ${password}

..
   Using this decorator without an argument will have no effect on the exposed
   keyword name, but will still create the `robot_name` attribute.  This can be useful
   for `Marking methods to expose as keywords`_ without actually changing
   keyword names.

인수없이 이 장식자를 사용하면 노출된 키워드 이름에 영향을 주지 않지만
여전히 `robot_name` 속서을 생성할 것이다. 이것은 실제로 키워드 이름을
변경하지 않고 `키워드로 노출하기 위한 메소드 표시 <Marking methods to
expose as keywords_>`__ 하는 데 유용하다.
   
..
   Setting a custom keyword name can also enable library keywords to accept
   arguments using `Embedded Arguments`__ syntax.

사용자 정의 키워드 이름을 설정하면 라이브러리 키워드가 `임베디드
전달인자`__ 문법을 사용하여 인자를 받아들일 수 있다.
   
__ `Embedding arguments into keyword names`_

Keyword tags
~~~~~~~~~~~~

..
   Starting from Robot Framework 2.9, library keywords and `user keywords`__ can
   have tags. Library keywords can define them by setting the `robot_tags`
   attribute on the method to a list of desired tags. The `robot.api.deco.keyword`
   decorator may be used as a shortcut for setting this attribute when used as
   follows:

Robot Framework 2.9부터 라이브러리 키워드와 `user keywords`__ 는
태그를 가질 수 있다. 라이브러리 키워드는 원하는 태그의 목록을 메소드의
`robot_tags` 속성으로 설정함으로써 태그를 정의 할 수 있다.
`robot.api.deco.keyword` 장식자는 다음과 같이 사용하는 경우에 이
속성을 설정하는 바로가기로 사용할 수 있다:
   
.. sourcecode:: python

  from robot.api.deco import keyword

  @keyword(tags=['tag1', 'tag2'])
  def login(username, password):
      # ...

  @keyword('Custom name', ['tags', 'here'])
  def another_example():
      # ...

..
   Another option for setting tags is giving them on the last line of
   `keyword documentation`__ with `Tags:` prefix and separated by a comma. For
   example:

태그 설정을 위한 다른 선택사항은 `키워드 문서`__ 의 마지막 라인에
`Tags:` 접두어와 쉼표로 구분하여 적을 수 있다. 예제 :
   
.. sourcecode:: python

  def login(username, password):
      """Log user in to SUT.

      Tags: tag1, tag2
      """
      # ...

__ `User keyword tags`_
__ `Documenting libraries`_

Keyword arguments
~~~~~~~~~~~~~~~~~

..
   With a static and hybrid API, the information on how many arguments a
   keyword needs is got directly from the method that implements it.
   Libraries using the `dynamic library API`_ have other means for sharing
   this information, so this section is not relevant to them.

정적 및 하이브리드 API에서 키워드에 얼마나 많은 인자가 필요한지에 대한
정보는 그것을 구현하는 메소드로 부터 직접 얻을 수 있다. `동적
라이브러리 API <dynamic library API_>`__ 는 이 정보를 공유하기 위한
다른 수단을 가지므로 이 섹션은 그와는 관련이 없다.
   
..
   The most common and also the simplest situation is when a keyword needs an
   exact number of arguments. In this case, both the Python and Java methods
   simply take exactly those arguments. For example, a method implementing a
   keyword with no arguments takes no arguments either, a method
   implementing a keyword with one argument also takes one argument, and
   so on.

가장 일반적이고 가장 간단한 상황은 키워드가 정확한 수의 인수를 필요로
하는 경우 이다. 이 경우 파이썬과 자바 메소드는 정확하게 이 인수를
가진다. 예를 들어 전달인자가 없는 키워드를 구현하는 메소드는
전달인자가 없고, 하나의 전달인자를 가지는 키워드를 구현하는 메소드는
하나의 전달인자를 가진다.

..
   Example Python keywords taking different numbers of arguments:

다른 수의 전달일자를 취하는 파이썬 키워드 예제:
   
.. sourcecode:: python

  def no_arguments():
      print "Keyword got no arguments."

  def one_argument(arg):
      print "Keyword got one argument '%s'." % arg

  def three_arguments(a1, a2, a3):
      print "Keyword got three arguments '%s', '%s' and '%s'." % (a1, a2, a3)

..
   .. note:: A major limitation with Java libraries using the static library API
	     is that they do not support the `named argument syntax`_. If this
	     is a blocker, it is possible to either use Python or switch to
	     the `dynamic library API`_.

.. note:: 정적 라이브러리 API를 사용하는 자바 라이브러리의 주요 제한은
          `명명 전달인자 문법 <named argument syntax_>`__ 를 지원하지
          않는다는 것이다. 이것이 문제라면 파이썬을 사용하거나 `동적
          라이브러리 API <dynamic library API_>`__ 를 사용하라.
	  
Default values to keywords
~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   It is often useful that some of the arguments that a keyword uses have
   default values. Python and Java have different syntax for handling default
   values to methods, and the natural syntax of these languages can be
   used when creating test libraries for Robot Framework.

키워드의 인수중 일부가 기본 값을 가지는 것은 종종 유용하다. 파이썬과
자바는 메소드에 기본값을 처리하기 위한 다른 문법을 가지고, 이런 언어의
자연 구문은 Robot Framework에 대한 테스트 라이브러리를 생성할 때
사용될 수 있다.
   
Default values with Python
''''''''''''''''''''''''''

..
   In Python a method has always exactly one implementation and possible
   default values are specified in the method signature. The syntax,
   which is familiar to all Python programmers, is illustrated below:

파이썬에서 메소드는 항상 정확하게 하나의 구현을 가지고 가능한 기본
값은 메소드 시그너처에 기술한다. 이 문법은 모든 파이썬 프로그래머에게
익숙하며, 아래에 설명되어 있다:
   
.. sourcecode:: python

   def one_default(arg='default'):
       print "Argument has value %s" % arg

   def multiple_defaults(arg1, arg2='default 1', arg3='default 2'):
       print "Got arguments %s, %s and %s" % (arg1, arg2, arg3)

..
   The first example keyword above can be used either with zero or one
   arguments. If no arguments are given, `arg` gets the value
   `default`. If there is one argument, `arg` gets that value,
   and calling the keyword with more than one argument fails. In the
   second example, one argument is always required, but the second and
   the third one have default values, so it is possible to use the keyword
   with one to three arguments.

위에서 첫번째 예제 키워드는 0개 또는 1개의 전달인자를 사용할 수 있다.
만약 전달인자가 주어지지 않는다면 `arg`는 `default` 값을 얻는다.
하나의 전달인자가 존재하면 `arg` 는 그 값을 얻고 하나 이상의
전달인자를 가지고 키워드를 호출하면 실패한다. 두번째 예제에서 항상
1개의 전달인자는 필요하다. 하지만 두번째, 세번째 전달인자는 기본값을
가지므로, 1개에서 3개의 전달인자를 가지는 키워드로 사용할 수 있다.
   
.. sourcecode:: robotframework

   *** Test Cases ***
   Defaults
       One Default
       One Default    argument
       Multiple Defaults    required arg
       Multiple Defaults    required arg    optional
       Multiple Defaults    required arg    optional 1    optional 2

Default values with Java
''''''''''''''''''''''''

..
   In Java one method can have several implementations with different
   signatures. Robot Framework regards all these implementations as one
   keyword, which can be used with different arguments. This syntax can
   thus be used to provide support for the default values. This is
   illustrated by the example below, which is functionally identical to
   the earlier Python example:

자바에서 한개의 메소드는 다른 시크너처를 가지는 여러 구현체를 만들 수
있다. Robot Framework는 모든 이 구현체를 다른 전달인자를 가지고 사용할
수 있는 하나의 키워드로 간주한다. 그래서 이 문법은 기본값을 지원하기
위해 사용될 수 있다. 아래 예제에서 이전의 파이썬 예제와 동일한 기능을
어떻게 구현하는지 설명한다:
   
.. sourcecode:: java

   public void oneDefault(String arg) {
       System.out.println("Argument has value " + arg);
   }

   public void oneDefault() {
       oneDefault("default");
   }

   public void multipleDefaults(String arg1, String arg2, String arg3) {
       System.out.println("Got arguments " + arg1 + ", " + arg2 + " and " + arg3);
   }

   public void multipleDefaults(String arg1, String arg2) {
       multipleDefaults(arg1, arg2, "default 2");
   }

   public void multipleDefaults(String arg1) {
       multipleDefaults(arg1, "default 1");
   }

Variable number of arguments (`*varargs`)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Robot Framework supports also keywords that take any number of
   arguments. Similarly as with the default values, the actual syntax to use
   in test libraries is different in Python and Java.

Robot Framework는 또한 임의 갯수의 전달인자를 가지는 키워드를
지원한다. 기본값을 가지는 것과 비슷하게, 테스트 라이브러리에서 사용하기
위한 실제 문법은 파이썬과 자바에서 다르다.

Variable number of arguments with Python
''''''''''''''''''''''''''''''''''''''''

..
   Python supports methods accepting any number of arguments. The same
   syntax works in libraries and, as the examples below show, it can also
   be combined with other ways of specifying arguments:

파이썬은 임의 갯수의 전달인자를 가지는 메소드를 지원한다. 동일한
문법이 라이브러리에서 동작한다. 아래 예제에서 이것을 보이고 전달인자를
지정하기 위한 다른 방법과 조합되는 것 또한 설명한다:

   
.. sourcecode:: python

  def any_arguments(*args):
      print "Got arguments:"
      for arg in args:
          print arg

  def one_required(required, *others):
      print "Required: %s\nOthers:" % required
      for arg in others:
          print arg

  def also_defaults(req, def1="default 1", def2="default 2", *rest):
      print req, def1, def2, rest

.. sourcecode:: robotframework

   *** Test Cases ***
   Varargs
       Any Arguments
       Any Arguments    argument
       Any Arguments    arg 1    arg 2    arg 3    arg 4    arg 5
       One Required     required arg
       One Required     required arg    another arg    yet another
       Also Defaults    required
       Also Defaults    required    these two    have defaults
       Also Defaults    1    2    3    4    5    6

Variable number of arguments with Java
''''''''''''''''''''''''''''''''''''''

..
   Robot Framework supports `Java varargs syntax`__ for defining variable number of
   arguments. For example, the following two keywords are functionally identical
   to the above Python examples with same names:

Robot Framework는 가변 인자에 대한 `Java varargs syntax`__ 를
지원한다. 예를 들어 다음 두 키워드는 위에서 보인 동일한 이름의 파이썬
예제와 기능적으로 동일하다.

.. sourcecode:: java

  public void anyArguments(String... varargs) {
      System.out.println("Got arguments:");
      for (String arg: varargs) {
          System.out.println(arg);
      }
  }

  public void oneRequired(String required, String... others) {
      System.out.println("Required: " + required + "\nOthers:");
      for (String arg: others) {
          System.out.println(arg);
      }
  }

..
   It is also possible to use variable number of arguments also by
   having an array or, starting from Robot Framework 2.8.3,
   `java.util.List` as the last argument, or second to last
   if `free keyword arguments (**kwargs)`_ are used. This is illustrated
   by the following examples that are functionally identical to
   the previous ones:

Robot Framework 2.8.3 부터 가변 인자 사용은 배열 또는 마지막 인자나
마지막 두번째 인자(마지막 인자 `free keyword arguments (**kwargs)`_ 가
사용될 때) `java.util.List` 을 사용할 수 있다. 다음 예제에서 설명하고
앞의 것과는 기능적으로 동일하다.

.. sourcecode:: java

  public void anyArguments(String[] varargs) {
      System.out.println("Got arguments:");
      for (String arg: varargs) {
          System.out.println(arg);
      }
  }

  public void oneRequired(String required, List<String> others) {
      System.out.println("Required: " + required + "\nOthers:");
      for (String arg: others) {
          System.out.println(arg);
      }
  }

..
   .. note:: Only `java.util.List` is supported as varargs, not any of
	     its sub types.

.. note:: 단지 `java.util.List` 는 임의의 하위 유형이 아닌 varargs 로 지원된다. 
	  
..
   The support for variable number of arguments with Java keywords has one
   limitation: it works only when methods have one signature. Thus it is not
   possible to have Java keywords with both default values and varargs.
   In addition to that, only Robot Framework 2.8 and newer support using
   varargs with `library constructors`__.

가변 인자를 가지는 자바 키워드에 대한 지원은 하나의 제한이 있다:
메소드들이 하나의 시그너처를 가질 경우만 동작한다. 그래서 기본값과
varargs 둘다 가지는 자바 키워드는 불가능하다. 그 뿐만 아니라 Robot
Framework 2.8 이후 버전만 `library constructors`__ 와 함께 varargs를
사용하는 것을 지원한다.
   
__ http://docs.oracle.com/javase/1.5.0/docs/guide/language/varargs.html
__ `Providing arguments to test libraries`_

Free keyword arguments (`**kwargs`)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Robot Framework 2.8 added the support for free keyword arguments using Python's
   `**kwargs` syntax. How to use the syntax in the test data is discussed
   in `Free keyword arguments`_ section under `Creating test cases`_. In this
   section we take a look at how to actually use it in custom test libraries.

Robot Framework 2.8부터 파이썬 `**kwargs` 문법을 사용하는 키워드
전달인자를 지원한다. 테스트 데이타에서 이 문법을 사용하는 방법은
`Creating test cases`_ 의 `Free Keyword arguments`_ 섹션에서 설명한다.
이 섹션에서 실제로 사용자 정의 테스트 라이브러리를 사용하는 방법을
살펴보자.

   
Free keyword arguments with Python
''''''''''''''''''''''''''''''''''

..
   If you are already familiar how kwargs work with Python, understanding how
   they work with Robot Framework test libraries is rather simple. The example
   below shows the basic functionality:

파이썬에서 어떻게 kwargs가 동작하는 지에 익숙하면 Robot Framework
테스트 라이브러리가 동작하는 것을 이해하는 것은 훨씬 간단하다. 아래
예제는 기본적인 기능을 보여준다:

.. sourcecode:: python

    def example_keyword(**stuff):
        for name, value in stuff.items():
            print name, value

.. sourcecode:: robotframework

   *** Test Cases ***
   Keyword Arguments
       Example Keyword    hello=world        # Logs 'hello world'.
       Example Keyword    foo=1    bar=42    # Logs 'foo 1' and 'bar 42'.

..
   Basically, all arguments at the end of the keyword call that use the
   `named argument syntax`_ `name=value`, and that do not match any
   other arguments, are passed to the keyword as kwargs. To avoid using a literal
   value like `foo=quux` as a free keyword argument, it must be escaped__
   like `foo\=quux`.

기본적으로 `명명 전달인자 문법 <named argument syntax_>`__
`name=value` 를 사용하고, 어떤 다른 전달인자와 일치하지 않는 키워드
호출의 끝에 있는 모든 전달인자는 키워드에 kwargs로 전달된다. 키워드
전달인자로 문자 그대로의 값 `foo=quux` 을 사용하는 것을 피하기
위해서는 `foo\=quux` 처럼 이스케이프__ 해야 한다.
   
..
   The following example illustrates how normal arguments, varargs, and kwargs
   work together:

다음 예제는 보통의 전달인자(normal arguments), 가변인자(varargs),
키워드 가변인자(kwargs)를 함께 사용하는 것을 설명한다:

.. sourcecode:: python

  def various_args(arg, *varargs, **kwargs):
      print 'arg:', arg
      for value in varargs:
          print 'vararg:', value
      for name, value in sorted(kwargs.items()):
          print 'kwarg:', name, value

.. sourcecode:: robotframework

   *** Test Cases ***
   Positional
       Various Args    hello    world                # Logs 'arg: hello' and 'vararg: world'.

   Named
       Various Args    arg=value                     # Logs 'arg: value'.

   Kwargs
       Various Args    a=1    b=2    c=3             # Logs 'kwarg: a 1', 'kwarg: b 2' and 'kwarg: c 3'.
       Various Args    c=3    a=1    b=2             # Same as above. Order does not matter.

   Positional and kwargs
       Various Args    1    2    kw=3                # Logs 'arg: 1', 'vararg: 2' and 'kwarg: kw 3'.

   Named and kwargs
       Various Args    arg=value      hello=world    # Logs 'arg: value' and 'kwarg: hello world'.
       Various Args    hello=world    arg=value      # Same as above. Order does not matter.

..
   For a real world example of using a signature exactly like in the above
   example, see :name:`Run Process` and :name:`Start Keyword` keywords in the
   Process_ library.

위의 예제 같이 시그너처를 사용하는 실세계 예제는 Process_ 라이브러리의
:name:`Run Process` 와 :name:`Start Keyword` 키워드를 참고하라.
   
__ Escaping_

Free keyword arguments with Java
''''''''''''''''''''''''''''''''

Starting from Robot Framework 2.8.3, also Java libraries support the free
keyword arguments syntax. Java itself has no kwargs syntax, but keywords
can have `java.util.Map` as the last argument to specify that they
accept kwargs.

If a Java keyword accepts kwargs, Robot Framework will automatically pack
all arguments in `name=value` syntax at the end of the keyword call
into a `Map` and pass it to the keyword. For example, following
example keywords can be used exactly like the previous Python examples:

.. sourcecode:: java

    public void exampleKeyword(Map<String, String> stuff):
        for (String key: stuff.keySet())
            System.out.println(key + " " + stuff.get(key));

    public void variousArgs(String arg, List<String> varargs, Map<String, Object> kwargs):
        System.out.println("arg: " + arg);
        for (String varg: varargs)
            System.out.println("vararg: " + varg);
        for (String key: kwargs.keySet())
            System.out.println("kwarg: " + key + " " + kwargs.get(key));

.. note:: The type of the kwargs argument must be exactly `java.util.Map`,
          not any of its sub types.

.. note:: Similarly as with the `varargs support`__, a keyword supporting
          kwargs cannot have more than one signature.

__ `Variable number of arguments with Java`_

Argument types
~~~~~~~~~~~~~~

..
   Normally keyword arguments come to Robot Framework as strings. If
   keywords require some other types, it is possible to either use
   variables_ or convert strings to required types inside keywords. With
   `Java keywords`__ base types are also coerced automatically.

보통 키워드 전달인자는 문자열로 Robot Framework에 온다. 키워드가 다른
유형을 필요로 하는 경우 variables_ 나 키워드 내부에서 요구되는
형식으로 문자열을 변환 할 수 있다. 또한 `자바 키워드`__ 기본 유형은
자동적으로 강제 변환된다.

__ `Argument types with Java`_

Argument types with Python
''''''''''''''''''''''''''

..
   Because arguments in Python do not have any type information, there is
   no possibility to automatically convert strings to other types when
   using Python libraries. Calling a Python method implementing a keyword
   with a correct number of arguments always succeeds, but the execution
   fails later if the arguments are incompatible. Luckily with Python it
   is simple to convert arguments to suitable types inside keywords:

파이썬에서 전달인자는 어떤 형정보도 가지지 않으므로 파이썬
라이브러리를 사용할 때 문자열을 다른 형식으로 자동적으로 변환할 수
없다. 정확한 수의 인수를 가지는 키워드를 구현하는 파이썬 메소드를
호출하는 것은 항상 성공하지만 이후에 전달인자가 호환되지 않는 다면
실행은 실패한다. 다행히 파이썬으로 키워드 내부에 적합한 유형의 인수로
변환하는 것은 간단하다:

.. sourcecode:: python

  def connect_to_host(address, port=25):
      port = int(port)
      # ...

Argument types with Java
''''''''''''''''''''''''

Arguments to Java methods have types, and all the base types are
handled automatically. This means that arguments that are normal
strings in the test data are coerced to correct type at runtime. The
types that can be coerced are:

- integer types (`byte`, `short`, `int`, `long`)
- floating point types (`float` and `double`)
- the `boolean` type
- object versions of the above types e.g. `java.lang.Integer`

The coercion is done for arguments that have the same or compatible
type across all the signatures of the keyword method. In the following
example, the conversion can be done for keywords `doubleArgument`
and `compatibleTypes`, but not for `conflictingTypes`.

.. sourcecode:: java

   public void doubleArgument(double arg) {}

   public void compatibleTypes(String arg1, Integer arg2) {}
   public void compatibleTypes(String arg2, Integer arg2, Boolean arg3) {}

   public void conflictingTypes(String arg1, int arg2) {}
   public void conflictingTypes(int arg1, String arg2) {}

The coercion works with the numeric types if the test data has a
string containing a number, and with the boolean type the data must
contain either string `true` or `false`. Coercion is only
done if the original value was a string from the test data, but it is
of course still possible to use variables containing correct types with
these keywords. Using variables is the only option if keywords have
conflicting signatures.

.. sourcecode:: robotframework

   *** Test Cases ***
   Coercion
       Double Argument     3.14
       Double Argument     2e16
       Compatible Types    Hello, world!    1234
       Compatible Types    Hi again!    -10    true

   No Coercion
       Double Argument    ${3.14}
       Conflicting Types    1       ${2}    # must use variables
       Conflicting Types    ${1}    2

Starting from Robot Framework 2.8, argument type coercion works also with
`Java library constructors`__.

__ `Providing arguments to test libraries`_

Using decorators
~~~~~~~~~~~~~~~~

..
   When writing static keywords, it is sometimes useful to modify them with
   Python's decorators. However, decorators modify function signatures,
   and can confuse Robot Framework's introspection when determining which
   arguments keywords accept. This is especially problematic when creating
   library documentation with Libdoc_ and when using  RIDE_. To avoid this
   issue, either do not use decorators, or use the handy `decorator module`__
   to create signature-preserving decorators.

정적 키워드를 작성할 때, 파이썬 장식자를 사용하여 수정하는 것은
유용하다. 그러나 장식자는 함수 시그너처를 수정하고, 키워드가 어떤
인수를 받아들이지를 결정할 때 Robot Framework의 내성(introspection)을
혼동 할 수 있다. 이는 특히 Libdoc_ 으로 라이브러리 문서를 만들때와
RIDE_ 를 사용할 때 문제가 된다. 이런 문제를 방지하려면 장식자를
사용하지 않거나 시그너처 보존 장식자를 만들기 위하여 편리한 `장식자
모듈`__ 을 사용해야 한다.

__ http://micheles.googlecode.com/hg/decorator/documentation.html

Embedding arguments into keyword names
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Library keywords can also accept arguments which are passed using
   `Embedded Argument syntax`__.  The `robot.api.deco.keyword` decorator
   can be used to create a `custom keyword name`__ for the keyword
   which includes the desired syntax.

라이브러리 키워드는 `임베디드 전달인자 문법`__ 을 사용하여 전달하는
전달인자를 받을 수 있다. `robot.api.deco.keyword` 장식자는 원하는
구문을 포함하는 키워드를 위해서 `사용자 정의 키워드 이름`__ 을 만들어
사용할 수 있다.
 
__ `Embedding arguments into keyword name`_
__ `Using a custom keyword name`_

.. sourcecode:: python

    from robot.api.deco import keyword

    @keyword('Add ${quantity:\d+} Copies Of ${item} To Cart')
    def add_copies_to_cart(quantity, item):
        # ...

.. sourcecode:: robotframework

   *** Test Cases ***
   My Test
       Add 7 Copies Of Coffee To Cart

Communicating with Robot Framework
----------------------------------

..
   After a method implementing a keyword is called, it can use any
   mechanism to communicate with the system under test. It can then also
   send messages to Robot Framework's log file, return information that
   can be saved to variables and, most importantly, report if the
   keyword passed or not.

키워드를 구현한 메소드가 호출 된 후, 메소드는 테스트중인 시스템(SUT)과
통신하기 위해서 임의의 메커니즘을 사용할 수 있다. 또한 메소드는 Robot
Framework의 로그 파일에 메시지를 보낼 수 있고, 변수에 저장할 수 있는
정보를 반환하고, 가장 중요하게 키워드가 패쓰 하거나 실패 한 경우
레포트 할 수 있다.
   
Reporting keyword status
~~~~~~~~~~~~~~~~~~~~~~~~

..
   Reporting keyword status is done simply using exceptions. If an executed
   method raises an exception, the keyword status is `FAIL`, and if it
   returns normally, the status is `PASS`.

키워드 상태를 레포팅하는 것은 단순히 예외를 사용하여 수행된다. 실행된
메소드가 예외를 발생하면 키워드 상태는 `FAIL` 이고 정상적으로 반환하면
`PASS` 상태이다.

..
   The error message shown in logs, reports and the console is created
   from the exception type and its message. With generic exceptions (for
   example, `AssertionError`, `Exception`, and
   `RuntimeError`), only the exception message is used, and with
   others, the message is created in the format `ExceptionType:
   Actual message`.

로그, 레포트 및 콘솔에 보여진 에러 메시지는 예외 유형과 그 메시지로
부터 생성된다. 일반적인 예외(예를 들어, `AssertionError`, `Exception`,
`RuntimeError`)만 예외 메시지를 사용하고, 나머지 예외에서 메시지는
`ExceptionType: Actual Message` 형식으로 생성한다.

..
   Starting from Robot Framework 2.8.2, it is possible to avoid adding the
   exception type as a prefix to failure message also with non generic exceptions.
   This is done by adding a special `ROBOT_SUPPRESS_NAME` attribute with
   value `True` to your exception.

Robot Framework 2.8.2 부터 비 일반 예외도 실패 메시지에 접두어로 예외
유형을 추가하여 회피할 수 있다. 이것은 당신의 예외에 특별한
`ROBOT_SUPPRESS_NAME` 속성이 `True` 값을 가지도록 추가하여 수행된다.
   
Python:

.. sourcecode:: python

    class MyError(RuntimeError):
        ROBOT_SUPPRESS_NAME = True

Java:

.. sourcecode:: java

    public class MyError extends RuntimeException {
        public static final boolean ROBOT_SUPPRESS_NAME = true;
    }

..
   In all cases, it is important for the users that the exception message is as
   informative as possible.

모든 경우에서 사용자는 예외 메시지가 최대한 유익한 정보를 가지도록
하는 것이 중요하다.

HTML in error messages
''''''''''''''''''''''

..
   Starting from Robot Framework 2.8, it is also possible have HTML formatted
   error messages by starting the message with text `*HTML*`:

Robot Framework 2.8부터 HTML 형식의 에러 메시지를 가질 수 있다. 이
메시지는 `*HTML*` 테스트로 시작한다:

.. sourcecode:: python

   raise AssertionError("*HTML* <a href='robotframework.org'>Robot Framework</a> rulez!!")

..
   This method can be used both when raising an exception in a library, like
   in the example above, and `when users provide an error message in the test data`__.

이 메소드는 위의 예제와 같이 라이브러리에서 예외가 발생할 때나
`사용자가 테스트 데이타에서 에러 메시지를 제공 할 때`__ 모두 사용할 수
있다.
   
__ `Failures`_

Cutting long messages automatically
'''''''''''''''''''''''''''''''''''

..
   If the error message is longer than 40 lines, it will be automatically
   cut from the middle to prevent reports from getting too long and
   difficult to read. The full error message is always shown in the log
   message of the failed keyword.

에러 메시지가 40라인보다 더 길다면 너무나 길고 읽기 어려운 리포트를
방지하기 위해서 자동적으로 중간에서 절단한다. 완전한 에러 메시지는
항상 실패된 키워드의 로그 메시지에서 볼 수 있다.

Tracebacks
''''''''''

..
   The traceback of the exception is also logged using `DEBUG` `log level`_.
   These messages are not visible in log files by default because they are very
   rarely interesting for normal users. When developing libraries, it is often a
   good idea to run tests using `--loglevel DEBUG`.

예외의 역추적(traceback)은 `DEBUG` `log level`_ 을 사용하여 기록된다.
보통의 사용자에게 매우 드물게 흥미를 유발하기 때문에 이런 메시지는
기본적으로 로그 파일에서 볼 수 없다. 라이브러리를 개발할 때
`--loglevel DEBUG` 를 사용하여 테스트를 실행하는 것은 좋은 아이디어다.


Stopping test execution
~~~~~~~~~~~~~~~~~~~~~~~

..
   It is possible to fail a test case so that `the whole test execution is
   stopped`__. This is done simply by having a special `ROBOT_EXIT_ON_FAILURE`
   attribute with `True` value set on the exception raised from the keyword.
   This is illustrated in the examples below.

test case 가 실패하면 `전체 테스트 실행이 중지된다`__. 이것은 간단하게
특별한 `ROBOT_EXIT_ON_FAILURE` 속성을 True로 설정하면 키워드로 부터
예외가 일어나면 행해진다. 이것은 아래 예제에서 설명한다.


Python:

.. sourcecode:: python

    class MyFatalError(RuntimeError):
        ROBOT_EXIT_ON_FAILURE = True

Java:

.. sourcecode:: java

    public class MyFatalError extends RuntimeException {
        public static final boolean ROBOT_EXIT_ON_FAILURE = true;
    }

__ `Stopping test execution gracefully`_

Continuing test execution despite of failures
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   It is possible to `continue test execution even when there are failures`__.
   The way to signal this from test libraries is adding a special
   `ROBOT_CONTINUE_ON_FAILURE` attribute with `True` value to the exception
   used to communicate the failure. This is demonstrated by the examples below.

`실패가 있더라도 테스트 수행을 계속하는 것`__ 이 가능하다. 테스트
라이브러리로부터 이것을 신호로 보내는 방법은 실패를 통신하는데
사용되는 예외에 `True` 값을 가지는 특별한 `ROBOT_CONTINUE_ON_FAILURE`
속성을 추가한다. 이것은 아래 예제에서 보여진다.

Python:

.. sourcecode:: python

    class MyContinuableError(RuntimeError):
        ROBOT_CONTINUE_ON_FAILURE = True

Java:

.. sourcecode:: java

    public class MyContinuableError extends RuntimeException {
        public static final boolean ROBOT_CONTINUE_ON_FAILURE = true;
    }

__ `Continue on failure`_

Logging information
~~~~~~~~~~~~~~~~~~~

..
   Exception messages are not the only way to give information to the
   users. In addition to them, methods can also send messages to `log
   files`_ simply by writing to the standard output stream (stdout) or to
   the standard error stream (stderr), and they can even use different
   `log levels`_. Another, and often better, logging possibility is using
   the `programmatic logging APIs`_.

예외 메시지만이 사용자에게 정보를 주는 유일한 방법은 아니다. 그 뿐만
아니라 메소드는 심지어 다른 `log levels`_ 을 사용하여, 간단하게 표준
출력 스트림(stdout) 또는 표준 에러 스트림(stderr) 적음으로써 메시지를
`log files`_ 에 보낼 수 있다. 또, 종종 더 나은 로깅 가능성이
`programmatic logging APIs`_ 를 사용한다.
   
..
   By default, everything written by a method into the standard output is
   written to the log file as a single entry with the log level
   `INFO`. Messages written into the standard error are handled
   similarly otherwise, but they are echoed back to the original stderr
   after the keyword execution has finished. It is thus possible to use
   the stderr if you need some messages to be visible on the console where
   tests are executed.

기본적으로 메소드에 의해 표준 출력으로 적혀지는 모든 것은 로그 레벨
`INFO` 를 가지는 당일 항목으로 로그 파일에 적혀진다. 표준 출력으로
적혀지는 메시지는 유사하게 처리되지만 키워드 실행이 완료되면 다시 원래
표준 오류로 반향된다. 그래서 테스트가 실행되는 콘솔에서 몇몇 메시지를
보기 원한다면 stderr을 사용할 수 있다.
   
Using log levels
''''''''''''''''

..
   To use other log levels than `INFO`, or to create several
   messages, specify the log level explicitly by embedding the level into
   the message in the format `*LEVEL* Actual log message`, where
   `*LEVEL*` must be in the beginning of a line and `LEVEL` is
   one of the available logging levels `TRACE`, `DEBUG`,
   `INFO`, `WARN`, `ERROR` and `HTML`.

`INFO` 보다 다른 로그 레벨을 사용하기 위해서나 여러가지 메시지를
생성하기 위해서는 명시적으로 로그 레벨을 `*LEVEL* Actual log message`
형식안에 삽입하여 지정해야 한다. 여기서 `*LEVEL*` 은 반드시 라인의
시작이어야 하고 `LEVEL`은 가용한 로깅 레벨 `TRACE`, `DEBUG`, `INFO`,
`WARN`, `ERROR`, `HTML` 중 하나이다.

Errors and warnings
'''''''''''''''''''

..
   Messages with `ERROR` or `WARN` level are automatically written to the
   console and a separate `Test Execution Errors section`__ in the log
   files. This makes these messages more visible than others and allows
   using them for reporting important but non-critical problems to users.

`ERROR` 또는 `WARN` 레벨을 가지는 메시지는 자동적으로 콘솔과 로그
파일내의 별도의 `테스트 실행 오류 절`__ 에 적힌다. 이것은 이런
메시지를 다른 것들보다 보다 눈에 뜨게 만들고 사용자에게 중요하지만
결정적이지 않은 문제를 리포팅하는데 사용할 수 있다.
   
..
   .. note:: In Robot Framework 2.9, new functionality was added to automatically
	     add ERRORs logged by keywords to the Test Execution Errors section.

.. note:: Robot Framework 2.9에서 자동적으로 키워드에 의해 기록되는
          ERROR를 테스트 실행 에러 색선에 추가하는 새로운 기능이
          추가되었다.
	     
__ `Errors and warnings during execution`_

Logging HTML
''''''''''''

..
   Everything normally logged by the library will be converted into a
   format that can be safely represented as HTML. For example,
   `<b>foo</b>` will be displayed in the log exactly like that and
   not as **foo**. If libraries want to use formatting, links, display
   images and so on, they can use a special pseudo log level
   `HTML`. Robot Framework will write these messages directly into
   the log with the `INFO` level, so they can use any HTML syntax
   they want. Notice that this feature needs to be used with care,
   because, for example, one badly placed `</table>` tag can ruin
   the log file quite badly.

일반적으로 라이브러리에 의해 기록되는 모든 것은 안전하게 HTML포 표현
될 수 있는 형식으로 변환된다. 예를 들어 `<b>foo</b>` 는 **foo** 가
아니라 정확히 같게 로그에 표시 될 것이다. 라이브러에서 형식, 링크,
이미지 표시 등등을 사용하기를 원한다면 특별한 의사 로그 수준 `HTML` 을
사용할 수 있다. Robot Framework은 이런 메시지를 직접 `INFO` 레벨로
로그에 적는다. 그래서 원하는 어떤 HTML 문법도 사용할 수 있다. 이
기능은 예를 들어, 잘못 놓인 `</table>` 태그가 로그 파일을 아주
엉망으로 망칠 수 있기 때문에 주의 하여 사용할 필요가 있다.

..
   When using the `public logging API`_, various logging methods
   have optional `html` attribute that can be set to `True`
   to enable logging in HTML format.

`Public logging API`_ 를 사용할 경우 여러가기 로깅 메소드는 추가적인
`html` 속성을 가진다. 이 속성은 HTML 형식으로 로깅하는 것을 가능하게
하기 위해서는 `True` 로 설정할 수 있다.
   
Timestamps
''''''''''

..
   By default messages logged via the standard output or error streams
   get their timestamps when the executed keyword ends. This means that
   the timestamps are not accurate and debugging problems especially with
   longer running keywords can be problematic.

기본적으로 표준 출력 또는 에러 스트림을 통해 기록되는 메시지는 실행된
키워드가 끝날 때 자신의 타임스탬프를 얻는다. 이것은 타임스탬프가
정확하지 않아서 특히 긴 실행시간을 가지는 키워드와 함께 할 때 문제를
디버깅하는 것은 문제가 될 수 있다.

..
   Keywords have a possibility to add an accurate timestamp to the messages
   they log if there is a need. The timestamp must be given as milliseconds
   since the `Unix epoch`__ and it must be placed after the `log level`__
   separated from it with a colon::

키워드는 필요하다면 기록하는 메시지에 정확한 타임스탬프를 추가 할 수
있다. 타임스탬프는 `Unix epoch`__ 이후로 밀리초 단위로 주어지고
콜론으로 분리된 `로그 레벨`__ 뒤에 위치해야 한다::
  
   *INFO:1308435758660* Message with timestamp
   *HTML:1308435758661* <b>HTML</b> message with timestamp

..
   As illustrated by the examples below, adding the timestamp is easy
   both using Python and Java. If you are using Python, it is, however,
   even easier to get accurate timestamps using the `programmatic logging
   APIs`_. A big benefit of adding timestamps explicitly is that this
   approach works also with the `remote library interface`_.

아래에 제시된 예제처럼, 타임스탬프를 더하는 것은 파이썬과 자바를
사용하는 두 경우 모두 쉽다. 그러나 파이썬을 사용하는 경우 `programmatic
logging APIs`_ 를 사용하여 정확한 타임스탬프를 얻는 것이 더 쉽다.
타임스탬프를 명시적으로 추가하는 것의 가장 큰 이점은 `remote library
interface`_ 역시 작동한다는 것이다.

Python:

.. sourcecode:: python

    import time

    def example_keyword():
        print '*INFO:%d* Message with timestamp' % (time.time()*1000)

Java:

.. sourcecode:: java

    public void exampleKeyword() {
        System.out.println("*INFO:" + System.currentTimeMillis() + "* Message with timestamp");
    }

__ http://en.wikipedia.org/wiki/Unix_epoch
__ `Using log levels`_

Logging to console
''''''''''''''''''

..
   If libraries need to write something to the console they have several
   options. As already discussed, warnings and all messages written to the
   standard error stream are written both to the log file and to the
   console. Both of these options have a limitation that the messages end
   up to the console only after the currently executing keyword
   finishes. A bonus is that these approaches work both with Python and
   Java based libraries.

라이브러리가 콘솔로 무언가를 쓸 필요가 있는 경우에는 몇가지 옵션이
있다. 이미 논의 된 바와 같이 경과와 표준 에러 스트림에 기록된 모든
메시지는 로그 파일과 콘솔에 둘다 적혀진다. 이런 옵션은 모두 현재
실행중인 키워드가 완료 된 후에 메시지가 콘솔에 끝낸다는 한계를 가진다.
보너스는 이러한 접근 방법은 파이썬과 자바 기반 라이브러리에서 모두
작동한다는 것이다.


..
   Another option, that is only available with Python, is writing
   messages to `sys.__stdout__` or `sys.__stderr__`. When
   using this approach, messages are written to the console immediately
   and are not written to the log file at all:

또 파이썬에서만 유효한 다른 옵션은 메시지를 `sys.__stdout__` 또는
`sys.__stderr__` 에 적는 것이다. 이 방법을 사용하는 경우 메시지는 즉시
콘솔에 적히고 로그 파일에는 전혀 기록되지 않는다:

.. sourcecode:: python

   import sys

   def my_keyword(arg):
      sys.__stdout__.write('Got arg %s\n' % arg)

..
   The final option is using the `public logging API`_:

마지막 옵션은 `public logging API`_ 를 사용한다:

.. sourcecode:: python

   from robot.api import logger

   def log_to_console(arg):
      logger.console('Got arg %s' % arg)

   def log_to_console_and_log_file(arg)
      logger.info('Got arg %s' % arg, also_console=True)

Logging example
'''''''''''''''

..
   In most cases, the `INFO` level is adequate. The levels below it,
   `DEBUG` and `TRACE`, are useful for writing debug information.
   These messages are normally not shown, but they can facilitate debugging
   possible problems in the library itself. The `WARN` or `ERROR` level can
   be used to make messages more visible and `HTML` is useful if any
   kind of formatting is needed.

대부분의 경우 `INFO` 레벨이 적절하다. 그 아래 수준 `DEBUG` 와 `TRACE`
는 디버그 정보를 기록하는데 유용하다. 일반적으로 이런 메시지는
보여지지 않지만 라이브러리 자체 디버깅 가능한 문제를 용이하게 할 수
있다. `WARN` 또는 `ERROR` 레벨은 메시지를 더 눈에 띄게 만들 수 있으며
`HTML` 은 임의의 형식이 필요한 경우 유용하다.

..
   The following examples clarify how logging with different levels
   works. Java programmers should regard the code `print 'message'`
   as pseudocode meaning `System.out.println("message");`.

다음 예제는 다른 수준으로 로깅하는 동작 방법을 설명한다. 자바
프로그래머는 코드 `print 'message'` 를 의사 의미
`System.out.println("message");` 로 생각해야 한다.

.. sourcecode:: python

   print 'Hello from a library.'
   print '*WARN* Warning from a library.'
   print '*ERROR* Something unexpected happen that may indicate a problem in the test.'
   print '*INFO* Hello again!'
   print 'This will be part of the previous message.'
   print '*INFO* This is a new message.'
   print '*INFO* This is <b>normal text</b>.'
   print '*HTML* This is <b>bold</b>.'
   print '*HTML* <a href="http://robotframework.org">Robot Framework</a>'

.. raw:: html

   <table class="messages">
     <tr>
       <td class="time">16:18:42.123</td>
       <td class="info level">INFO</td>
       <td class="msg">Hello from a library.</td>
     </tr>
     <tr>
       <td class="time">16:18:42.123</td>
       <td class="warn level">WARN</td>
       <td class="msg">Warning from a library.</td>
     </tr>
     <tr>
       <td class="time">16:18:42.123</td>
       <td class="error level">ERROR</td>
       <td class="msg">Something unexpected happen that may indicate a problem in the test.</td>
     </tr>
     <tr>
       <td class="time">16:18:42.123</td>
       <td class="info level">INFO</td>
       <td class="msg">Hello again!<br>This will be part of the previous message.</td>
     </tr>
     <tr>
       <td class="time">16:18:42.123</td>
       <td class="info level">INFO</td>
       <td class="msg">This is a new message.</td>
     </tr>
     <tr>
       <td class="time">16:18:42.123</td>
       <td class="info level">INFO</td>
       <td class="msg">This is &lt;b&gt;normal text&lt;/b&gt;.</td>
     </tr>
     <tr>
       <td class="time">16:18:42.123</td>
       <td class="info level">INFO</td>
       <td class="msg">This is <b>bold</b>.</td>
     </tr>
     <tr>
       <td class="time">16:18:42.123</td>
       <td class="info level">INFO</td>
       <td class="msg"><a href="http://robotframework.org">Robot Framework</a></td>
     </tr>
   </table>

Programmatic logging APIs
~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Programmatic APIs provide somewhat cleaner way to log information than
   using the standard output and error streams. Currently these
   interfaces are available only to Python bases test libraries.

프로그래밍 API는 표준 출력과 에러 스트림을 사용하는 것보다 정보를
로깅하기위한 다소 깨끗한 방법을 제공한다. 현재 이러한 인터페이스는
파이썬 기초 테스트 라이브러리만 사용할 수 있다.

Public logging API
''''''''''''''''''

..
   Robot Framework has a Python based logging API for writing
   messages to the log file and to the console. Test libraries can use
   this API like `logger.info('My message')` instead of logging
   through the standard output like `print '*INFO* My message'`. In
   addition to a programmatic interface being a lot cleaner to use, this
   API has a benefit that the log messages have accurate timestamps_.

Robot Framework은 메시지를 로그 파일과 콘솔에 기록하기 위한 파이썬에
기초한 로깅 API 가진다. 테스트 라이브러리는 `print '*INFO* My
message'` 처럼 표준 출력을 통해 로깅하는 대신에 `logger.info('My
message')` 처럼 이 API를 사용할 수 있다. 많은 더 깨끗한 사용을 위한
프로그래밍 인터페이스 이외에 이 API는 로그 메시지가 정확한 `타임스탬프
<timestamps_>`__ 를 가진다는 이점이 있다.

..
   The public logging API `is thoroughly documented`__ as part of the API
   documentation at https://robot-framework.readthedocs.org. Below is
   a simple usage example:

공개 로깅 API는 https://robot-framework.readthedocs.org 에 API 문서의
일부로 `철저히 문서화`__ 한다. 아래는 간단한 사용 예제이다:

.. sourcecode:: python

   from robot.api import logger

   def my_keyword(arg):
       logger.debug('Got argument %s' % arg)
       do_something()
       logger.info('<i>This</i> is a boring example', html=True)
       logger.console('Hello, console!')

..
   An obvious limitation is that test libraries using this logging API have
   a dependency to Robot Framework. Before version 2.8.7 Robot also had
   to be running for the logging to work. Starting from Robot Framework 2.8.7
   if Robot is not running the messages are redirected automatically to Python's
   standard logging__ module.

분명한 제한은 이 로깅 API를 사용하는 테스트 라이브러리는 Robot
Framework에 의존성을 가진다는 것이다. 버전 2.8.7 이전의 Robot은 또한
로깅을 동작하기 위해서는 실행되어야 했다. Robot Framework 2.8.7 부터
Robot이 실행되지 않으면 메시지는 자동적으로 파이썬 표준 로깅__ 모듈로
재전송된다.

__ https://robot-framework.readthedocs.org/en/latest/autodoc/robot.api.html#module-robot.api.logger
__ http://docs.python.org/library/logging.html

Using Python's standard `logging` module
''''''''''''''''''''''''''''''''''''''''

..
   In addition to the new `public logging API`_, Robot Framework offers a
   built-in support to Python's standard logging__ module. This
   works so that all messages that are received by the root logger of the
   module are automatically propagated to Robot Framework's log
   file. Also this API produces log messages with accurate timestamps_,
   but logging HTML messages or writing messages to the console are not
   supported. A big benefit, illustrated also by the simple example
   below, is that using this logging API creates no dependency to Robot
   Framework.

새로운 `public logging API`_ 외에도 Robot Framework는 파이썬 표준
로깅__ 모듈에 내장 된 지원을 제공한다. 이것은 모듈의 루트 로거에 의에
수신된 모든 메시지가 자동적으로 Robot Framework의 로그 파일로 전달
되도록 동작한다. 또한 이 API는 정확한 timestamps_ 와 로그 메시지를
생성하지만, HTML 메시지를 로깅하거나 콘솔에 메시지를 쓰는 것은
지원되지 않는다. 아래의 간단한 예제로 설명되는 큰 이점은 이 로깅 API는
Robot Framework에 대한 종속성을 생성하지 않는다는 것이다.

.. sourcecode:: python

   import logging

   def my_keyword(arg):
       logging.debug('Got argument %s' % arg)
       do_something()
       logging.info('This is a boring example')

..
   The `logging` module has slightly different log levels than
   Robot Framework. Its levels `DEBUG`, `INFO`, `WARNING` and `ERROR` are mapped
   directly to the matching Robot Framework log levels, and `CRITICAL`
   is mapped to `ERROR`. Custom log levels are mapped to the closest
   standard level smaller than the custom level. For example, a level
   between `INFO` and `WARNING` is mapped to Robot Framework's `INFO` level.

`logging` 모듈은 Robot Framework보다 약간 다른 로그 레벨을 가진다.
`DEBUG`, `INFO`, `WARNING`, `ERROR` 레벨이 직접 Robot Framework 로그
레벨과 매핑하고, `CRITICAL` 은 `ERROR` 에 매핑된다. 사용자 정의 로그
레벨은 사용자 정의 레벨보다 약간 작은 가장 가까운 표준 레벨에
매핑된다. 예를 들어 `INFO` 와 `WARNING` 사이의 레벨은 Robot
Framework의 `INFO` 레벨에 매핑된다.


__ http://docs.python.org/library/logging.html

Logging during library initialization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Libraries can also log during the test library import and initialization.
   These messages do not appear in the `log file`_ like the normal log messages,
   but are instead written to the `syslog`_. This allows logging any kind of
   useful debug information about the library initialization. Messages logged
   using the `WARN` or `ERROR` levels are also visible in the `test execution errors`_
   section in the log file.

라이브러리는 테스트 라이브러리 임포트 및 초기화 동안 로깅할 수 있다.
이런 메시지는 정상적인 로그 메시지 같은 `log file`_ 에 표시되지 않지만
대신 `syslog`_ 에 기록된다. 이것은 라이브러리 초기화에 대한 다양하고
유용한 디버그 정보를 로깅할 수 있다. `WARN` 또는 `ERROR` 레벨을
사용하여 로깅된 메시지는 `테스트 실행 에러 <test execution errors_>`__
에 표시될 수 있다.

..
   Logging during the import and initialization is possible both using the
   `standard output and error streams`__ and the `programmatic logging APIs`_.
   Both of these are demonstrated below.

임포트와 초기화하는 동안 로깅하는 것은 `표준 출력 및 에러 스트림`__ 과
`programmatic logging APIs`_ 를 사용하여 가능하다. 이 두가지에 대한
예제를 아래에서 보인다.

..
   Java library logging via stdout during initialization:

초기화 동안 stdout을 통해 로깅하는 자바 라이브러리:

.. sourcecode:: java

   public class LoggingDuringInitialization {

       public LoggingDuringInitialization() {
           System.out.println("*INFO* Initializing library");
       }

       public void keyword() {
           // ...
       }
   }

..
   Python library logging using the logging API during import:

임포트 하는 동안 로깅 API를 사용하여 로깅하는 파이썬 라이브러리:
   
.. sourcecode:: python

   from robot.api import logger

   logger.debug("Importing library")

   def keyword():
       # ...

..
   .. note:: If you log something during initialization, i.e. in Python
	     `__init__` or in Java constructor, the messages may be
	     logged multiple times depending on the `test library scope`_.

.. note:: 파이썬 `__init__` 또는 자바 생성자를 사용하는 초기화 동안
          무언가를 기록하려면 메시지는 `테스트 라이브러리 범위 <test
          library scope_>`__ 에 따라 여러번 로깅 될 수 있다.
	  
__ `Logging information`_

Returning values
~~~~~~~~~~~~~~~~

..
   The final way for keywords to communicate back to the core framework
   is returning information retrieved from the system under test or
   generated by some other means. The returned values can be `assigned to
   variables`__ in the test data and then used as inputs for other keywords,
   even from different test libraries.

키워드가 다시 코어 프레임워크와 통신하기 위한 최종 방법은 테스트 중인
시스템으로 부터 받거나 어떤 다른 수단에 의해 생성된 정보를 반환하는
것이다. 반환된 값은 테스트 데이타에서 `변수에 할당`__ 하여 다른
키워드(심지어 다른 테스트 라이브러리에 속한)에 대하여 입력으로 사용할
수 있다.

..
   Values are returned using the `return` statement both from
   the Python and Java methods. Normally, one value is assigned into one
   `scalar variable`__, as illustrated in the example below. This example
   also illustrates that it is possible to return any objects and to use
   `extended variable syntax`_ to access object attributes.

값은 파이썬과 자바 메소드로 부터 `return` 문을 사용하여 반환된다.
일반적으로 아래 예제와 같이 하나의 값은 하나의 `스칼라 변수`__ 에 할당
된다. 이 예제는 또한 임의의 객체를 리턴하여 객체 속성에 접근할 수 있는
`확장 변수 문법 <extended variable syntax_>`__ 을 사용할 수 있다는
것을 보여준다.

__ `Return values from keywords`_
__ `Scalar variables`_

.. sourcecode:: python

  from mymodule import MyObject

  def return_string():
      return "Hello, world!"

  def return_object(name):
      return MyObject(name)

.. sourcecode:: robotframework

   *** Test Cases ***
   Returning one value
       ${string} =    Return String
       Should Be Equal    ${string}    Hello, world!
       ${object} =    Return Object    Robot
       Should Be Equal    ${object.name}    Robot

..
   Keywords can also return values so that they can be assigned into
   several `scalar variables`_ at once, into `a list variable`__, or
   into scalar variables and a list variable. All these usages require
   that returned values are Python lists or tuples or
   in Java arrays, Lists, or Iterators.

키워드는 값들을 리턴하여 한번에 여러개의 `스칼라 변수 <scalar
variables_>`__ 나 `하나의 리스트 변수`__ 에 할당할 수 있다. 이런
사용법은 리턴된 값들이 파이썬 리스트, 튜풀이거나 자바 배열, 리스트,
반복자(Iterators) 이어야 한다.

__ `List variables`_

.. sourcecode:: python

  def return_two_values():
      return 'first value', 'second value'

  def return_multiple_values():
      return ['a', 'list', 'of', 'strings']


.. sourcecode:: robotframework

   *** Test Cases ***
   Returning multiple values
       ${var1}    ${var2} =    Return Two Values
       Should Be Equal    ${var1}    first value
       Should Be Equal    ${var2}    second value
       @{list} =    Return Two Values
       Should Be Equal    @{list}[0]    first value
       Should Be Equal    @{list}[1]    second value
       ${s1}    ${s2}    @{li} =    Return Multiple Values
       Should Be Equal    ${s1} ${s2}    a list
       Should Be Equal    @{li}[0] @{li}[1]    of strings

Communication when using threads
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   If a library uses threads, it should generally communicate with the
   framework only from the main thread. If a worker thread has, for
   example, a failure to report or something to log, it should pass the
   information first to the main thread, which can then use exceptions or
   other mechanisms explained in this section for communication with the
   framework.

라이브러리가 스레드를 사용할 경우 일반적으로 단지 주(main) 스레드만
프레임워크와 통신해야 한다. 예를 들어, 작업자(worker) 스레드가 실패를
보고하거나 어떤 것을 로깅하려면, 예외 또는 프레임워크와 통신하기 위한
이 절에서 설명하는 다른 메카니즘을 사용하여 주 스레드에 먼저 정보를
전달해야 한다.

..
   This is especially important when threads are run on background while
   other keywords are running. Results of communicating with the
   framework in that case are undefined and can in the worst case cause a
   crash or a corrupted output file. If a keyword starts something on
   background, there should be another keyword that checks the status of
   the worker thread and reports gathered information accordingly.

이것은 특히 스레드가 다른 키워드가 동작하고 있는 동안 백그라운드로
수행될 때 중요하다. 이 경우 프레임워크와 통신 결과는 정의되지 않고
최악의 경우 크래시나 출력 파일 손상을 일으킬 수 있다. 키워드가
백그라운드로 시작하면, 다른 키워드는 작업자 스레드의 상태를 확인하고
그것에 따라 수집된 정보를 보고해야 한다.

..
   Messages logged by non-main threads using the normal logging methods from
   `programmatic logging APIs`_  are silently ignored.

주가 아닌(non-main) 스레드에 의해 `Programmatic logging APIs`_ 의 정상
로깅 메소드를 사용하여 로깅된 메시지는 소리없이(silently) 무시된다.
   
..
   There is also a `BackgroundLogger` in separate robotbackgroundlogger__ project,
   with a similar API as the standard `robot.api.logger`. Normal logging
   methods will ignore messages from other than main thread, but the
   `BackgroundLogger` will save the background messages so that they can be later
   logged to Robot's log.

표준 `robot.api.logger` 와 유사한 API로 별도의 robotbackgroundlogger__
프로젝트의 `BackgroundLogger` 가 존재한다. 일반 로깅 메소드는 주
스레드 이외의 스레드가 생성하는 메시지를 무시하지만,
`BackgroundLogger` 는 백그라운드 메시지를 저장해서 나중에 Robot의
로그에 기록할 수 있다.
   
__ https://github.com/robotframework/robotbackgroundlogger

Distributing test libraries
---------------------------

Documenting libraries
~~~~~~~~~~~~~~~~~~~~~

..
   A test library without documentation about what keywords it
   contains and what those keywords do is rather useless. To ease
   maintenance, it is highly recommended that library documentation is
   included in the source code and generated from it. Basically, that
   means using docstrings_ with Python and Javadoc_ with Java, as in
   the examples below.

테스트 라이브러리가 포함하는 키워드와 그 키워드가 무엇을 하는지에 대한
문서가 없다면 오히려 쓸모 없다. 유지보수를 용이하게 하기 위해
라이브러리 문서는 소스 코드에 포함되고 그것으로 부터 생성되게 해야
한다. 기본적으로 아래 예처럼 파이썬의 docstrings_ 과 자바의 Javadoc_
을 사용해야 한다.

.. sourcecode:: python

    class MyLibrary:
        """This is an example library with some documentation."""

        def keyword_with_short_documentation(self, argument):
            """This keyword has only a short documentation"""
            pass

        def keyword_with_longer_documentation(self):
            """First line of the documentation is here.

            Longer documentation continues here and it can contain
            multiple lines or paragraphs.
            """
            pass

.. sourcecode:: java

    /**
     *  This is an example library with some documentation.
     */
    public class MyLibrary {

        /**
         * This keyword has only a short documentation
         */
        public void keywordWithShortDocumentation(String argument) {
        }

        /**
         * First line of the documentation is here.
         *
         * Longer documentation continues here and it can contain
         * multiple lines or paragraphs.
         */
        public void keywordWithLongerDocumentation() {
        }

    }

..
   Both Python and Java have tools for creating an API documentation of a
   library documented as above. However, outputs from these tools can be slightly
   technical for some users. Another alternative is using Robot
   Framework's own documentation tool Libdoc_. This tool can
   create a library documentation from both Python and Java libraries
   using the static library API, such as the ones above, but it also handles
   libraries using the `dynamic library API`_ and `hybrid library API`_.

위처럼 파이썬과 자바는 문서화된 라이브러리 API 문서를 생성하기 위한
툴을 가진다. 그러나 이런 툴의 출력물은 일부 사용자에게는 다소
기술적이다. 다른 대안은 Robot Framework의 문서화 툴 Libdoc_ 을
사용하는 것이다. 이 툴은 상술한 것과 같은 정적 라이브러리 API를
사용하는 파이썬과 자바 라이브러리로부터 라이브러리 문서를 생성한다.
또한 `동적 라이브러리 API <dynamic library API_>`__ 와 `하이브리드
라이브러리 API <hybrid library API_>`__ 를 사용하는 라이브러리도 다룰
수 있다.

..
   The first line of a keyword documentation is used for a special
   purpose and should contain a short overall description of the
   keyword. It is used as a *short documentation*, for example as a tool
   tip, by Libdoc_ and also shown in the test logs. However, the latter
   does not work with Java libraries using the static API,
   because their documentations are lost in compilation and not available
   at runtime.

키워드 문서의 첫 번째 라인은 특별한 목적으로 사용되며, 키워드 전체에
대한 짧은 설명을 포함한다. 그것은 Libdoc_ 에 의해 툴팁에 사용하는
*짧은 문서* 로 사용될 수 있고, 또한 테스트 로그에도 보여진다. 그러나
후자는 정적 API를 사용하는 자바 라이브러리와는 작동하지 않는다.
왜냐하면 소스의 문서가 컴파일때 없어지고 실행시간에는 사용할 수 없다.

..
   By default documentation is considered to follow Robot Framework's
   `documentation formatting`_ rules. This simple format allows often used
   styles like `*bold*` and `_italic_`, tables, lists, links, etc.
   Starting from Robot Framework 2.7.5, it is possible to use also HTML, plain
   text and reStructuredText_ formats. See `Specifying documentation format`_
   section for information how to set the format in the library source code and
   Libdoc_ chapter for more information about the formats in general.

기본적으로 문서는 Robot Framework의 `문서 형식 <documentation
formatting_>`__ 를 따른다고 간주된다. 이 간단한 형식은 종종 `*bold*`,
`_italic_`, 표, 리스트, 링크 등과 같은 스타일 사용을 허용한다. Robot
Framework 2.7.5 부터 HTML, 일반 텍스트, reStructuredText 형식 또한
사용할 수 있다. 라이브러리 소스 코드에 형식을 지정하는 방법에 대한
정보는 `문서 형식 지정 <Specifying documentation format_>`__ 절을
일반적으로 형식에 대한 더 자세한 정보는 Libdoc_ 장을 참고하라.

..
   .. note:: If you want to use non-ASCII characters in the documentation of
	     Python libraries, you must either use UTF-8 as your `source code
	     encoding`__ or create docstrings as Unicode.

.. note:: 파이썬 라이브러리의 문서에 non-ASCII 문자를 사용하려면
          UTF-8을 `소스 코드 인코딩`__ 으로 사용하거나 유니 코드로
          docstrings를 생성해야 한다.

.. _docstrings: http://www.python.org/dev/peps/pep-0257
.. _javadoc: http://java.sun.com/j2se/javadoc/writingdoccomments/index.html
__ http://www.python.org/dev/peps/pep-0263

Testing libraries
~~~~~~~~~~~~~~~~~

..
   Any non-trivial test library needs to be thoroughly tested to prevent
   bugs in them. Of course, this testing should be automated to make it
   easy to rerun tests when libraries are changed.

중요한 테스트 라이브러리는 버그를 방지하기 위해서 철저히 테스트 되어야
할 필요가 있다. 물론 이 테스팅은 라이브러리가 변경되었을 때 쉽게
재수행할 수 있도록 자동화되어야 한다.

..
   Both Python and Java have excellent unit testing tools, and they suite
   very well for testing libraries. There are no major differences in
   using them for this purpose compared to using them for some other
   testing. The developers familiar with these tools do not need to learn
   anything new, and the developers not familiar with them should learn
   them anyway.

파이썬과 자바는 우수한 유닛 테스팅 툴을 가지고 테스트 라이브러리에
아주 적합하다. 다른 테스팅을 위해서 사용하는 것에 비해 이 목적을 위해
이것을 사용하는 것은 별 차이가 없다.이런 툴에 친근한 개발자는 새로운
것을 배울 필요가 없지만 익숙하지 않은 개발자는 어쨌든 배워야 한다.

..
   It is also easy to use Robot Framework itself for testing libraries
   and that way have actual end-to-end acceptance tests for them. There are
   plenty of useful keywords in the BuiltIn_ library for this
   purpose. One worth mentioning specifically is :name:`Run Keyword And Expect
   Error`, which is useful for testing that keywords report errors
   correctly.

라이브러리를 테스팅하기 위한 Robot framework 자체는 사용하기 용이하고,
그 방법은 실제 라이브러리를 위한 종단간 인수 테스트를 가진다. 이런
목적을 위하여 BuiltIn_ 라이브러리에 유용한 키워드가 많이 있다. 특별히
언급할 가치가 있는 한 가지는 :name:`Run Keyword And Expect Error` 로
키워드가 에러를 올바르게 보고하는지를 테스팅하는데 유용하다.

..
   Whether to use a unit- or acceptance-level testing approach depends on
   the context. If there is a need to simulate the actual system under
   test, it is often easier on the unit level. On the other hand,
   acceptance tests ensure that keywords do work through Robot
   Framework. If you cannot decide, of course it is possible to use both
   the approaches.

유닛 또는 인수 레벨 테스팅 접근 방법은 상황(context)에 따라 사용할 수
있다. 테스트 중인 실제 시스템을 시뮬레이션 할 필요가 있다면 유닛
레벨이 좀 더 쉽다. 반면에 인수 테스트는 키워드가 Robot Framework를
통해 작동하는지를 확인한다. 결정할 수 없다면 당연히 두 방법 모두
사용할 수 있다.

Packaging libraries
~~~~~~~~~~~~~~~~~~~

..
   After a library is implemented, documented, and tested, it still needs
   to be distributed to the users. With simple libraries consisting of a
   single file, it is often enough to ask the users to copy that file
   somewhere and set the `module search path`_ accordingly. More
   complicated libraries should be packaged to make the installation
   easier.

라이브러리가 구현, 문서화, 테스트 된 후에는 사용자에게 배포 될 필요가
있다. 하나의 파일로 구성된 간단한 라이브러리라면 종종 그 파일을
어딘가에 복사하고 적절히 `모듈 검색 경로 <module search path_>`__
설정하도록 사용자에게 알려주는 것만으로 충분하다. 더 복잡한
라이브러리는 설치가 쉽도로 패키지로 만들어야 한다.

..
   Since libraries are normal programming code, they can be packaged
   using normal packaging tools. With Python, good options include
   distutils_, contained by Python's standard library, and the newer
   setuptools_. A benefit of these tools is that library modules are
   installed into a location that is automatically in the `module
   search path`_.

라이브러리는 정상적인 프로그래밍 코드이므로 패키징 툴을 사용하여
패키징 될 수 있다. 파이썬에는 좋은 옵션으로 파이썬 표준 라이브러리에
포함된 distutils_ 와 새로운 setuptools_ 가 포함되어 있다. 이런 툴의
장점은 라이브러리 모듈이 자동적으로 `모듈 검색 경로 <module search
path_>`__ 에 포함되는 위치에 설치된다는 것이다.

..
   When using Java, it is natural to package libraries into a JAR
   archive. The JAR package must be put into the `module search path`_
   before running tests, but it is easy to create a `start-up script`_ that
   does that automatically.

자바를 사용하면, JAR 아카이브로 라이브러리를 패키징하는 것은
자연스럽다. JAR 패키지는 테스트를 수행하기 전에 `모듈 검색 경로
<module search path_>`__ 에 놓여져야 하지만, 자동으로 작업을 수행하는
`시작 스크립트 <start-up script_>`__ 를 만드는 것이 쉽다.

Deprecating keywords
~~~~~~~~~~~~~~~~~~~~

..
   Sometimes there is a need to replace existing keywords with new ones
   or remove them altogether. Just informing the users about the change
   may not always be enough, and it is more efficient to get warnings at
   runtime. To support that, Robot Framework has a capability to mark
   keywords *deprecated*. This makes it easier to find old keywords from
   the test data and remove or replace them.

때로는 존재하는 키워드를 새로운 것으로 교체하거나 아예 제거 할 필요가
있다. 사용자에게 변경에 대한 정보를 주는 것만으로 충분하지 않으며,
런타임에 경고를 주는 것이 더욱 효과적이다. 이것을 지원하기 위해서
Robot Framework는 키워드에 *deprecated* 를 표시하는 기능이 있다.
이렇게 하면, 테스트 데이타에서 오래된 키워드를 찾아서 제거하거나 교체
할 수 있다.

..
   Keywords can be deprecated by starting their documentation with text
   `*DEPRECATED`, case-sensitive, and having a closing `*` also on the first
   line of the documentation. For example, `*DEPRECATED*`, `*DEPRECATED.*`, and
   `*DEPRECATED in version 1.5.*` are all valid markers.

키워드는 자신의 문서 첫라인에서 `*DEPRECATED` 로 시작하고, 대소문자를
구분하고 , `*` 로 닫히는 텍스트를 사용하여 deprecated 할 수 있다.
예를 들어 `*DEPRECATED*`, `*DEPRECATED.*`, `*DEPRECATED in version
1.5.*` 모든 적법한 표시다.

..
   When a deprecated keyword is executed, a deprecation warning is logged and
   the warning is shown also in `the console and the Test Execution Errors
   section in log files`__. The deprecation warning starts with text `Keyword
   '<name>' is deprecated.` and has rest of the `short documentation`__ after
   the deprecation marker, if any, afterwards. For example, if the following
   keyword is executed, there will be a warning like shown below in the log file.

Deprecated 키워드가 실행될 때, deprecation 경고가 로깅되고 `콘솔과
로그 파일내 테스트 수행 에러 섹션`__ 에 보여진다. deprecation 경고는
`Keyword '<name>' is deprecated.` 텍스트로 시작하고, deprecation
표식(있는 경우) 이후에 `짧은 문서`__ 의 나머지를 가진다. 예를 들어
다음 키워드가 실행될 경우 로그파일에 아래 와 같은 경고가 있을 것이다.


.. sourcecode:: python

    def example_keyword(argument):
        """*DEPRECATED!!* Use keyword `Other Keyword` instead.

        This keyword does something to given ``argument`` and returns results.
        """
        return do_something(argument)

.. raw:: html

   <table class="messages">
     <tr>
       <td class="time">20080911&nbsp;16:00:22.650</td>
       <td class="warn level">WARN</td>
       <td class="msg">Keyword 'SomeLibrary.Example Keyword' is deprecated. Use keyword `Other Keyword` instead.</td>
     </tr>
   </table>

..
   This deprecation system works with most test libraries and also with
   `user keywords`__.  The only exception are keywords implemented in a
   Java test library that uses the `static library interface`__ because
   their documentation is not available at runtime. With such keywords,
   it possible to use user keywords as wrappers and deprecate them.

이 deprecation 시스템은 대부분의 테스트 라이브러리와 `user keywords`__
와 작동한다. 유일한 예외는 문서를 런타임에 사용할 수 없는 `정적
라이브러리 인터페이스`__ 를 사용하는 자바 테스트 라이브러리에 구현된
키워드다. 그런 키워드의 경우 래퍼로 user keywords를 사용하여
deprecate할 수 있다.
   
..
   .. note:: Prior to Robot Framework 2.9 the documentation must start with
	     `*DEPRECATED*` exactly without any extra content before the
	     closing `*`.

.. note:: Robot Framework 2.9 이전 버전의 문서는 정확하게 닫는 `*`
          앞에 추가적인 내용 없이 `*DEPRECATED*` 를 사용해야 한다.

__ `Errors and warnings during execution`_
__ `Documenting libraries`_
__ `User keyword name and documentation`_
__ `Creating static keywords`_

.. _Dynamic library:

Dynamic library API
-------------------

..
   The dynamic API is in most ways similar to the static API. For
   example, reporting the keyword status, logging, and returning values
   works exactly the same way. Most importantly, there are no differences
   in importing dynamic libraries and using their keywords compared to
   other libraries. In other words, users do not need to know what APIs their
   libraries use.

동적 API는 정적 API와 가장 비슷한 방법이다. 예를 들면, 키워드의 상태를
보고, 로깅, 값을 리턴하는 것은 정확히 같은 방법으로 동작한다. 가장
중요한 것은 동적 라이브러리를 임포팅하고 다른 라이브러리에 비교해서
키우드를 사용하는데 차이가 없다. 즉 사용자는 라이브러리가 어떤 API를
사용하는지 알 필요가 없다.

..
   Only differences between static and dynamic libraries are
   how Robot Framework discovers what keywords a library implements,
   what arguments and documentation these keywords have, and how the
   keywords are actually executed. With the static API, all this is
   done using reflection (except for the documentation of Java libraries),
   but dynamic libraries have special methods that are used for these
   purposes.

단지 정적과 동적 라이브러리는 Robot Framework가 라이브러리에 무슨
키워드가 구현되었는지, 이 키워드가 무슨 인수와 문서를 가지는지,
키워드가 어떻게 실행되는지를 발견하는 방법에 차이가 있다. 정적 API를
가지면 이 모든 것은 *리플렉션* (자바 라이브러리의 문서는 예외)을
사용하지만 동적 라이브러리는 이런 목적으로 사용되는 *특별한 메소드* 를
가진다.

..
   One of the benefits of the dynamic API is that you have more flexibility
   in organizing your library. With the static API, you must have all
   keywords in one class or module, whereas with the dynamic API, you can,
   for example, implement each keyword as a separate class. This use case is
   not so important with Python, though, because its dynamic capabilities and
   multi-inheritance already give plenty of flexibility, and there is also
   possibility to use the `hybrid library API`_.

동적 API의 장점 중 하나는 더 유연하게 라이브러리를 구성할 수 있다는
것이다. 정적 API는 하나의 클래스나 모듈내에 모든 키워드를 가져야
하지만, 반면에 동적 API는 별개의 클래스로 각 키워드를 구현할 수 있다.
클래스의 동적 기능과 다중 상속은 이미 많은 유연성을 제공하고, 또한
`하이브리드 라이브러리 API <hybrid library API_>`__ 를 사용할 수 있는
가능성도 있기 때문에 이런 유즈 케이스는 파이썬에서 그렇게 중요하지
않다.

..
   Another major use case for the dynamic API is implementing a library
   so that it works as proxy for an actual library possibly running on
   some other process or even on another machine. This kind of a proxy
   library can be very thin, and because keyword names and all other
   information is got dynamically, there is no need to update the proxy
   when new keywords are added to the actual library.

동적 API의 다른 주요한 유즈 케이스는 다른 프로세스나 심지어 다른
머신에서 동작할 수 있는 실제 라이브러리의 프록시로 동작하는
라이브러리를 구현한다. 이런 종류의 프록시 라이브러리는 매우 얇을 수
있다. 왜냐하면 키워드 이름과 모든 다른 정보를 동적으로 얻을 수 있기
때문에, 새로운 키워드가 실제 라이브러리에 추가되었을 때 프록시를
업데이트 할 필요가 없기 때문이다.

..
   This section explains how the dynamic API works between Robot
   Framework and dynamic libraries. It does not matter for Robot
   Framework how these libraries are actually implemented (for example,
   how calls to the `run_keyword` method are mapped to a correct
   keyword implementation), and many different approaches are
   possible. However, if you use Java, you may want to examine
   `JavalibCore <https://github.com/robotframework/JavalibCore>`__
   before implementing your own system. This collection of
   reusable tools supports several ways of creating keywords, and it is
   likely that it already has a mechanism that suites your needs.

이 섹션은 정적 API가 Robot Framework와 동적 라이브러리사이에서
동작하는 방법을 설명한다. 이런 라이브러리가 실제로 구현된 방법(예를
들어 정확한 키워드 구현에 매핑되는 `run keyword` 메소드 호출 방법)은
Robot Framework에게는 문제가 되지 않으며 많은 다른 접근 방법이
가능하다. 그러나 자바를 사용 할 경우 자신의 시스템을 구현하기 전에
`JavalibCore <https://github.com/robotframework/JavalibCore>`__ 를
검사 할 수 있다. 이 재사용 가능한 툴은 키워드를 생성하는 여러가지
방법을 지원하고 이미 적절한 메카니즘을 가지고 있다.


.. _`Getting dynamic keyword names`:

Getting keyword names
~~~~~~~~~~~~~~~~~~~~~

..
   Dynamic libraries tell what keywords they implement with the
   `get_keyword_names` method. The method also has the alias
   `getKeywordNames` that is recommended when using Java. This
   method cannot take any arguments, and it must return a list or array
   of strings containing the names of the keywords that the library implements.

동적 라이브러리는 `get_keyword_names` 메소드를 구현하여 무슨 키워드가
있는지 알려준다. 이 메소드는 또한 자바를 사용할 때 추천되는
`getKeywordNames` 별칭을 가진다. 이 메소드는 어떤 인수도 가지지 않으며
반드시 라이브러리가 구현하는 키워드의 이름을 포함하는 리스트나 스트링
배열을 반환해야 한다.

..
   If the returned keyword names contain several words, they can be returned
   separated with spaces or underscores, or in the camelCase format. For
   example, `['first keyword', 'second keyword']`,
   `['first_keyword', 'second_keyword']`, and
   `['firstKeyword', 'secondKeyword']` would all be mapped to keywords
   :name:`First Keyword` and :name:`Second Keyword`.

반환된 키워드 이름이 여러 단어를 포함하는 경우, 공백이나 언더스코어
또는 camelCase 형식을 가지고 분리된채로 반환된다. 예를 들어, `['first
keyword', 'second keyword']`, `['first_keyword', 'second_keyword']`,
`['firstKeyword', 'secondKeyword']` 는 모두 키워드 :name:`First
Keyword` 와 :name:`Second Keyword` 에 매핑된다.
      
..
   Dynamic libraries must always have this method. If it is missing, or
   if calling it fails for some reason, the library is considered a
   static library.

동적 라이브러리는 항상 이 메소드를 가진다. 이것을 누락한 경우나 어떤
이유로 호출에 실패한 경우 라이브러리는 정적 라이브러리로 간주된다.

   
Marking methods to expose as keywords
'''''''''''''''''''''''''''''''''''''

..
   If a dynamic library should contain both methods which are meant to be keywords
   and methods which are meant to be private helper methods, it may be wise to
   mark the keyword methods as such so it is easier to implement `get_keyword_names`.
   The `robot.api.deco.keyword` decorator allows an easy way to do this since it
   creates a custom `robot_name` attribute on the decorated method.
   This allows generating the list of keywords just by checking for the `robot_name`
   attribute on every method in the library during `get_keyword_names`.  See
   `Using a custom keyword name`_ for more about this decorator.

동적 라이브러리는 키워드가 될 메소드와 개인적인 헬퍼 페소드가 될
메소드 모두 포함해야 하는 경우 키워드 메소드를 표시하기 위해서
`get_keyword_names` 를 구현하는 것이 더 현명하다.
`robot.api.deco.keyword` 장식자는 사용자 정의 `robot_name` 속성을
메소드에 장식하여 더 편하게 이것을 구현 할 수 있다. 이것은 단지
`get_keyword_names` 에서 라이브러리내의 모든 메소드의 `robot_name`
속성을 확인하여 키워드 목록을 생성 할 수 있다. 장식자에 대한 더 자세한
정보는 `사용자 정의 키워드 이름의 사용 <Using a custom keyword
name_>`__ 를 참고하라.

.. sourcecode:: python

   from robot.api.deco import keyword

   class DynamicExample:

       def get_keyword_names(self):
           return [name for name in dir(self) if hasattr(getattr(self, name), 'robot_name')]

       def helper_method(self):
           # ...

       @keyword
       def keyword_method(self):
           # ...

.. _`Running dynamic keywords`:

Running keywords
~~~~~~~~~~~~~~~~

..
   Dynamic libraries have a special `run_keyword` (alias
   `runKeyword`) method for executing their keywords. When a
   keyword from a dynamic library is used in the test data, Robot
   Framework uses the library's `run_keyword` method to get it
   executed. This method takes two or three arguments. The first argument is a
   string containing the name of the keyword to be executed in the same
   format as returned by `get_keyword_names`. The second argument is
   a list or array of arguments given to the keyword in the test data.

동적 라이브러리는 자신의 키워드를 실행하기 위한 특별한 `run_keyword`
(별칭 `runKeyword`) 메소드를 가진다. 동적 라이브러리로부터 키워드가
테스트 데이타에 사용될 경우, Robot Framework는 키워드를 얻어서
실행하기 위해 라이브러리의 `run_keyword` 메소드를 사용한다. 이
메소드는 두 개 또는 세 개의 전달인자를 취한다. 첫번째 전달인자는
`get_keyword_names` 에 의해 반환되는 것과 동일한 형식으로 실행되는
키워드의 이름을 포함하는 문자열이다. 두번째 전달인자는 테스트 데이타의
키워드에 주어진 전달인자의 리스트나 배열이다.

..
   The optional third argument is a dictionary (map in Java) that gets
   possible `free keyword arguments`_ (`**kwargs`) passed to the
   keyword. See `free keyword arguments with dynamic libraries`_ section
   for more details about using kwargs with dynamic test libraries.

세번째 선택적 전달인자는 사전(자바에서는 맵)으로 키워드에 전달 가능한
`키워드 인수 <free keyword arguments_>`__ (`**kwargs`)이다. 동적
테스트 라이브러리와 kwags를 사용하는 것에 대한 더 자세한 내용은 `동적
키워드 라이브러리와 키워드 인자 <free keyword arguments with dynamic
libraries_>`__ 섹션을 참고하라.

..
   After getting keyword name and arguments, the library can execute
   the keyword freely, but it must use the same mechanism to
   communicate with the framework as static libraries. This means using
   exceptions for reporting keyword status, logging by writing to
   the standard output or by using provided logging APIs, and using
   the return statement in `run_keyword` for returning something.

키워드 이름과 전달인자를 얻고 난 후에, 라이브러리는 자유롭게 키워드를
실행할 수 있지만 정적라이브러리처럼 프레임워크와 통신하기 위해 동일한
메카니즘을 사용해야 한다. 이것은 키워드 상태를 보고를 위해 예외를
사용하고, 표준 출력에 적거나 제공된 로깅 API를 사용하여 로깅하거나,
어떤것을 반환하기 위해 `run_keyword` 에서 return 문을 사용한다는 것을
의미한다.

..
   Every dynamic library must have both the `get_keyword_names` and
   `run_keyword` methods but rest of the methods in the dynamic
   API are optional. The example below shows a working, albeit
   trivial, dynamic library implemented in Python.

모든 동적 라이브러리는 `get_keyword_names` 와 `run_keyword` 메소드를
가져야 하지만 동적 API에서 나머지 메소드는 선택사항이다. 아래 예제는
파이썬으로 구현된 동적 라이브러리의 동작을 보여준다.

.. sourcecode:: python

   class DynamicExample:

       def get_keyword_names(self):
           return ['first keyword', 'second keyword']

       def run_keyword(self, name, args):
           print "Running keyword '%s' with arguments %s." % (name, args)

Getting keyword arguments
~~~~~~~~~~~~~~~~~~~~~~~~~

..
   If a dynamic library only implements the `get_keyword_names` and
   `run_keyword` methods, Robot Framework does not have any information
   about the arguments that the implemented keywords need. For example,
   both :name:`First Keyword` and :name:`Second Keyword` in the example above
   could be used with any number of arguments. This is problematic,
   because most real keywords expect a certain number of keywords, and
   under these circumstances they would need to check the argument counts
   themselves.

동적 라이브러리가 단지 `get_keywords_names` 와 `run_keyword` 메소드만
구현하면, Robot Framework는 구현된 키워드가 필요로하는 전달인자에 대한
어떤 정보도 가지지 못한다. 예를 들어 위 예제의 :name:`First Keyword`
와 :name:`Second Keyword` 는 임의 갯수의 전달인자와 사용할 수 있다.
이것은 대부분의 실제 키워드는 정해진 수의 키워드를 개대하기 때문에
문제가 있으며, 이러한 상황에서 키워드는 스스로 전달인자 갯수를 확인할
필요가 있다.

..
   Dynamic libraries can tell Robot Framework what arguments the keywords
   it implements expect by using the `get_keyword_arguments`
   (alias `getKeywordArguments`) method. This method takes the name
   of a keyword as an argument, and returns a list or array of strings
   containing the arguments accepted by that keyword.

동적 라이브러리는 Robot Framework에게 구현한 키워드가 무슨 전달인자를
기대하는지 `get_keyword_arguments` (별칭 `getKeywordArguments`)
메소드를 사용하여 알려준다. 이 메소드는 전달인자로 키워드 이름을
사용하고, 키워드가 받아들이는 전달인자를 포함하는 리스트나 문자열
배열을 반환한다.

..
   Similarly as static keywords, dynamic keywords can require any number
   of arguments, have default values, and accept variable number of
   arguments and free keyword arguments. The syntax for how to represent
   all these different variables is explained in the following table.
   Note that the examples use Python syntax for lists, but Java developers
   should use Java lists or String arrays instead.

정적 키워드와 비슷하게 동적 키워드는 기본값을 가지는 임의 갯수의
전달인자를 요구하고 가변인자 및 키워드 전달인자를 받을 수 있다. 모든
다른 변수를 나타내는 방법에 대한 문법은 아래 표에서 설명한다. 리스트에
대한 파이썬 문법을 사용하지만 자바 개발자는 대신에 자바 리스트와
문자열 배열을 사용해야 한다.

..
   .. table:: Representing different arguments with `get_keyword_arguments`
      :class: tabular

      +--------------------+----------------------------+------------------------------+----------+
      |    Expected        |      How to represent      |            Examples          | Limits   |
      |    arguments       |                            |                              | (min/max)|
      +====================+============================+==============================+==========+
      | No arguments       | Empty list.                | | `[]`                       | | 0/0    |
      +--------------------+----------------------------+------------------------------+----------+
      | One or more        | List of strings containing | | `['one_argument']`         | | 1/1    |
      | argument           | argument names.            | | `['a1', 'a2', 'a3']`       | | 3/3    |
      +--------------------+----------------------------+------------------------------+----------+
      | Default values     | Default values separated   | | `['arg=default value']`    | | 0/1    |
      | for arguments      | from names with `=`.       | | `['a', 'b=1', 'c=2']`      | | 1/3    |
      |                    | Default values are always  |                              |          |
      |                    | considered to be strings.  |                              |          |
      +--------------------+----------------------------+------------------------------+----------+
      | Variable number    | Last (or second last with  | | `['*varargs']`             | | 0/any  |
      | of arguments       | kwargs) argument has `*`   | | `['a', 'b=42', '*rest']`   | | 1/any  |
      | (varargs)          | before its name.           |                              |          |
      +--------------------+----------------------------+------------------------------+----------+
      | Free keyword       | Last arguments has         | | `['**kwargs']`             | | 0/0    |
      | arguments (kwargs) | `**` before its name.      | | `['a', 'b=42', '**kws']`   | | 1/2    |
      |                    |                            | | `['*varargs', '**kwargs']` | | 0/any  |
      +--------------------+----------------------------+------------------------------+----------+

.. table:: Representing different arguments with `get_keyword_arguments`
   :class: tabular
	   
   +--------------------+-----------------------------+------------------------------+----------+
   |    예상되는        |      표시 방법              |            예제              | 범위     |
   |    전달인자        |                             |                              | (min/max)|
   +====================+=============================+==============================+==========+
   | 전달인자 없음      | 빈 리스트.                  | | `[]`                       | | 0/0    |
   +--------------------+-----------------------------+------------------------------+----------+
   | 한개 또는 그 이상  | 전달 인자 이름을 포함하는   | | `['one_argument']`         | | 1/1    |
   | 전달인자           | 문자열 리스트               | | `['a1', 'a2', 'a3']`       | | 3/3    |
   +--------------------+-----------------------------+------------------------------+----------+
   | 기본값을 가지는    | `=` 로 구분되는 기본값.     | | `['arg=default value']`    | | 0/1    |
   | 전달인자           | 기본값은 항상 문자열로      | | `['a', 'b=1', 'c=2']`      | | 1/3    |
   |                    | 간주함.                     |                              |          |
   +--------------------+-----------------------------+------------------------------+----------+
   | 가변인자           | 이름앞에 `*` 를 가지는      | | `['*varargs']`             | | 0/any  |
   | (varargs)          | 마지막(또는 kwargs를 가질   | | `['a', 'b=42', '*rest']`   | | 1/any  |
   |                    | 경우 두번째 마지막)전달인자.|                              |          |
   +--------------------+-----------------------------+------------------------------+----------+
   | 키워드 전달인자    | 이름앞에 `**` 를 가지는     | | `['**kwargs']`             | | 0/0    |
   | (kwargs          ) | 마지막 전달인자.            | | `['a', 'b=42', '**kws']`   | | 1/2    |
   |                    |                             | | `['*varargs', '**kwargs']` | | 0/any  |
   +--------------------+-----------------------------+------------------------------+----------+

..
   When the `get_keyword_arguments` is used, Robot Framework automatically
   calculates how many positional arguments the keyword requires and does it
   support free keyword arguments or not. If a keyword is used with invalid
   arguments, an error occurs and `run_keyword` is not even called.

`get_keyword_arguments` 가 사용될 경우,Robot Framework는 자동적으로
키워드가 얼마나 많은 위치 전달인자를 요구하는지 계산하여 키워드
전달인자를 지원해야 할지 말지를 판단한다. 유효하지 않은 전달인자를
사용하는 경우, 오류가 발생하고 `run_keyword` 조차 호출되지 않는다.

..
   The actual argument names and default values that are returned are also
   important. They are needed for `named argument support`__ and the Libdoc_
   tool needs them to be able to create a meaningful library documentation.

반환되는 실제 전달인자 이름과 기본값도 중요하다. `명명 전달인자
지원`__ 할 필요가 있고, Libdoc_ 툴은 의미있는 라이브러리 문서를 생성할
수 있어야 한다.
   
..
   If `get_keyword_arguments` is missing or returns `None` or
   `null` for a certain keyword, that keyword gets an argument specification
   accepting all arguments. This automatic argument spec is either
   `[*varargs, **kwargs]` or `[*varargs]`, depending does
   `run_keyword` `support kwargs`__ by having three arguments or not.

`get_keyword_arguments` 는 어떤 키워드에 대해 없거나 `None` 또는
`null` 을 리턴하면, 키워드는 모든 전달인자를 받아들이는 인수를 얻는다.
이 자동 전달인자 스펙은 `[*varargs, **kwargs]` 나 `[*varargs]` 가
되고, 세개의 인수를 가지는지 여부에 의해 `run_keyword` 가 `kwargs
지원`__ 할 지를 결정한다.

__ `Named argument syntax with dynamic libraries`_
__ `Free keyword arguments with dynamic libraries`_

Getting keyword documentation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   The final special method that dynamic libraries can implement is
   `get_keyword_documentation` (alias
   `getKeywordDocumentation`). It takes a keyword name as an
   argument and, as the method name implies, returns its documentation as
   a string.

동적 라이브러리가 구현할 수 있는 마지막 특별한 메소드는
`get_keyword_documentation` (별칭 `getKeywordDocumentation`)이다.
이것은 키워드 이름을 전달인자로 받는다. 메소드 이름에서 알 수 있듯이
문자열로 키워드의 문서를 반환한다.

..
   The returned documentation is used similarly as the keyword
   documentation string with static libraries implemented with
   Python. The main use case is getting keywords' documentations into a
   library documentation generated by Libdoc_. Additionally,
   the first line of the documentation (until the first `\n`) is
   shown in test logs.

반환된 문서는 파이썬으로 구현된 정적 라이브러리의 키워드 문서
문자열처럼 사용된다. 주요 사용 사례는 Libdoc_ 로 생성한 라이브러리
문서속의 키워드 문서에서 사용된다. 또한 문서의 첫라인(첫번째 `\n`
까지)은 테스트 로그에 보여진다.

Getting keyword tags
~~~~~~~~~~~~~~~~~~~~

..
   Dynamic libraries do not have any other way for defining `keyword tags`_
   than by specifying them on the last row of the documentation with `Tags:`
   prefix. Separate `get_keyword_tags` method can be added to the dynamic API
   later if there is a need.

동적 라이브러리는 문서의 마지막 열에 `Tags:` 접두어를 가지고 명기하는
것외에 `키워드 태크 <keyword tags_>`__ 를 정의하는 다른 방법이 없다.
별도의 `get_keyword_tags` 메소드로 나중에 필요하다면 동적 API에 추가
할 수 있다.

Getting general library documentation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   The `get_keyword_documentation` method can also be used for
   specifying overall library documentation. This documentation is not
   used when tests are executed, but it can make the documentation
   generated by Libdoc_ much better.

`get_keyword_documentation` 메소드는 전체 라이브러리 문서를 명기하는데
사용할 수 있다. 이 문서는 테스트가 실행될 때 사용되지 않지만, Libdoc_
에 의해 생성되는 문서를 훨씬 더 좋게 만들 수 있다.

..
   Dynamic libraries can provide both general library documentation and
   documentation related to taking the library into use. The former is
   got by calling `get_keyword_documentation` with special value
   `__intro__`, and the latter is got using value
   `__init__`. How the documentation is presented is best tested
   with Libdoc_ in practice.

동적 라이브러리는 일반적인 라이브러리 문서 및 라이브러리를 사용에
관련된 문서를 제공할 수 있다. 전자는 특별한 값 `__intro__` 와
`get_keyword_documentation` 를 호출하여 얻을 수 있고, 후자는
`__init__` 값으로 얻을 수 있다. 문서가 표시되는 것은 실제로 Libdoc_ 에
의해 테스트된다.

..
   Python based dynamic libraries can also specify the general library
   documentation directly in the code as the docstring of the library
   class and its `__init__` method. If a non-empty documentation is
   got both directly from the code and from the
   `get_keyword_documentation` method, the latter has precedence.

파이썬 기반 동적 라이브러리는 또한 일반적인 라이브러리 문서를 코드에서
라이브러리 클래스의 docstring 및 그것의 `__init__` 메소드를 사용하여
명기할 수 있다. 비어 있지 않은 문서가 코드와
`get_keyword_documentation` 메소드로 부터 직접 모두 얻는다면 후자는
우선 순위를 가진다.


Named argument syntax with dynamic libraries
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Starting from Robot Framework 2.8, also the dynamic library API supports
   the `named argument syntax`_. Using the syntax works based on the
   argument names and default values `got from the library`__ using the
   `get_keyword_arguments` method.

Robot Framework 2.8 부터 동적 라이브러리 API는 `명명된 전달인자 문법
<named argument syntax_>`__ 을 지원한다. 문법을 사용하는 것은 인자
이름에 기초하여 동작하고, 기본값은 `get_keyword_arguments` 메소드를
사용하여 `라이브러리로 부터 얻을`__ 수 있다.

..
   For the most parts, the named arguments syntax works with dynamic keywords
   exactly like it works with any other keyword supporting it. The only special
   case is the situation where a keyword has multiple arguments with default
   values, and only some of the latter ones are given. In that case the framework
   fills the skipped optional arguments based on the default values returned
   by the `get_keyword_arguments` method.

대부분의 경우, 명명된 전달인자 문법은 동적 키워드와 함께 잘 작동한다.
유일한 특별한 경우는 키워드가 기본값을 가지는 여러개의 전달인자를
가지고, 뒤의 몇개만 주어진 상황이다. 이런 경우 프레임워크는
`get_keyword_arguments` 메소드의 반환된 기본 값들을 기초로 건더 뛴
옵션 전달인자를 채운다.

..
   Using the named argument syntax with dynamic libraries is illustrated
   by the following examples. All the examples use a keyword :name:`Dynamic`
   that has been specified to have argument specification
   `[arg1, arg2=xxx, arg3=yyy]`.
..
   The comment shows the arguments that the keyword is actually called with.

동적 라이브러리와 명명된 인자를 사용하는 것은 아래 예제에서 설명한다.
모든 예제는 전달인자 스펙 `[arg1, arg2=xxx, arg3=yyy]` 를 가지는
키워드 :name:`Dynamic` 을 사용한다. 주석은 키워드가 실제 호출될 때
가지는 전달인자이다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Only positional
       Dynamic    a                             # [a]
       Dynamic    a         b                   # [a, b]
       Dynamic    a         b         c         # [a, b, c]

   Named
       Dynamic    a         arg2=b              # [a, b]
       Dynamic    a         b         arg3=c    # [a, b, c]
       Dynamic    a         arg2=b    arg3=c    # [a, b, c]
       Dynamic    arg1=a    arg2=b    arg3=c    # [a, b, c]

   Fill skipped
       Dynamic    a         arg3=c              # [a, xxx, c]

__ `Getting keyword arguments`_

Free keyword arguments with dynamic libraries
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Starting from Robot Framework 2.8.2, dynamic libraries can also support
   `free keyword arguments`_ (`**kwargs`). A mandatory precondition for
   this support is that the `run_keyword` method `takes three arguments`__:
   the third one will get kwargs when they are used. Kwargs are passed to the
   keyword as a dictionary (Python) or Map (Java).

Robot Framework 2.8.2 부터 동적 라이브러리는 `자유 키워드 전달인자
<free keyword arguments_>`__ (`**kwargs`) 를 지원한다. 이런 지원을
위한 필수 전제 조건은 `run_keyword` 메소드가 `세개의 전달인자를
취한다`__ 는 것이다: 사용될 때 세번째는 kwargs로 얻을 것이다. Kwargs는
키워드에 사전(파이썬) 또는 맵(자바)로 전달 된다.

..
   What arguments a keyword accepts depends on what `get_keyword_arguments`
   `returns for it`__. If the last argument starts with `**`, that keyword is
   recognized to accept kwargs.

무슨 전달인자를 키워드가 수락하는지는 `get_keyword_arguments` 가
무엇을 `반환하는지`__ 에 달려 있다. 마지막 전달인자가 `**` 으로
시작하면 키워드는 kwargs를 받을 수 있는 것으로 인식된다.

..
   Using the free keyword argument syntax with dynamic libraries is illustrated
   by the following examples. All the examples use a keyword :name:`Dynamic`
   that has been specified to have argument specification
   `[arg1=xxx, arg2=yyy, **kwargs]`.
   The comment shows the arguments that the keyword is actually called with.

동적 라이브러리와 함께 자유 키워드 인자 문법을 사용하는 것은 아래
예제에서 보여준다. 모든 예제는 전달인자 스펙 `[arg1=xxx, arg2=yyy,
**kwargs]` 을 가지는 키워드 :name:`Dynamic` 을 사용한다. 주석은
키워드가 실제 호출될 때 가지는 전달인자이다.

.. sourcecode:: robotframework

   *** Test Cases ***
   No arguments
       Dynamic                            # [], {}

   Only positional
       Dynamic    a                       # [a], {}
       Dynamic    a         b             # [a, b], {}

   Only kwargs
       Dynamic    a=1                     # [], {a: 1}
       Dynamic    a=1       b=2    c=3    # [], {a: 1, b: 2, c: 3}

   Positional and kwargs
       Dynamic    a         b=2           # [a], {b: 2}
       Dynamic    a         b=2    c=3    # [a], {b: 2, c: 3}

   Named and kwargs
       Dynamic    arg1=a    b=2           # [a], {b: 2}
       Dynamic    arg2=a    b=2    c=3    # [xxx, a], {b: 2, c: 3}

__ `Running dynamic keywords`_
__ `Getting keyword arguments`_

Summary
~~~~~~~

..
   All special methods in the dynamic API are listed in the table
   below. Method names are listed in the underscore format, but their
   camelCase aliases work exactly the same way.

동적 API의 모든 특별한 메소드를 아래 표에 나열한다. 나열된 메소드
이름은 언더스코어 형식이지만 camelCase 별칭도 정확하게 동일한 방법으로
작동한다.

.. table:: All special methods in the dynamic API
   :class: tabular

   ===========================  =========================  =======================================================
               Name                    Arguments                                  Purpose
   ===========================  =========================  =======================================================
   `get_keyword_names`                                     `Return names`__ of the implemented keywords.
   `run_keyword`                `name, arguments, kwargs`  `Execute the specified keyword`__ with given arguments. `kwargs` is optional.
   `get_keyword_arguments`      `name`                     Return keywords' `argument specifications`__. Optional method.
   `get_keyword_documentation`  `name`                     Return keywords' and library's `documentation`__. Optional method.
   ===========================  =========================  =======================================================

__ `Getting dynamic keyword names`_
__ `Running dynamic keywords`_
__ `Getting keyword arguments`_
__ `Getting keyword documentation`_

..
   It is possible to write a formal interface specification in Java as
   below. However, remember that libraries *do not need* to implement
   any explicit interface, because Robot Framework directly checks with
   reflection if the library has the required `get_keyword_names` and
   `run_keyword` methods or their camelCase aliases. Additionally,
   `get_keyword_arguments` and `get_keyword_documentation`
   are completely optional.

아래 처럼 자바에서 형식적 인터페이스 스펙을 적을 수 있다. 하지만
라이브러리는 명시적인 인터페이스를 구현할 *필요가 없다* 는 것을
기억하라. 왜냐하면 Robot Framework는 라이브러리가 `get_keyword_names`
와 `run_keyword` 메소드나 그들의 camelCase 별칭이 요구한다면 직접
반영(reflection)으로 검사할 것이다. 게다가 `get_keyword_arguments` 와
`get_keyword_documentation` 은 완전히 선택사항이다.

.. sourcecode:: java

   public interface RobotFrameworkDynamicAPI {

       List<String> getKeywordNames();

       Object runKeyword(String name, List arguments);

       Object runKeyword(String name, List arguments, Map kwargs);

       List<String> getKeywordArguments(String name);

       String getKeywordDocumentation(String name);

   }

..
   .. note:: In addition to using `List`, it is possible to use also arrays
	     like `Object[]` or `String[]`.

.. note:: `List` 를 사용하는 것 이외에 `Object[]` 나 `String[]` 같은
          배열 또한 사용할 수 있다.
	  
..
   A good example of using the dynamic API is Robot Framework's own
   `Remote library`_.

동적 API를 사용하는 좋은 예제는 Robot Framework의 `Remote library`_
이다.

Hybrid library API
------------------

..
   The hybrid library API is, as its name implies, a hybrid between the
   static API and the dynamic API. Just as with the dynamic API, it is
   possible to implement a library using the hybrid API only as a class.

하이브리드 라이브러리 API는 그 이름이 의미하듯 정적 API와 동적 API
사이의 하이브리드 이다. 단지 동적 API 같이 클래스로서 이브리드 API를
사용하여 라이브러리를 구현할 수 있다.

Getting keyword names
~~~~~~~~~~~~~~~~~~~~~

..
   Keyword names are got in the exactly same way as with the dynamic
   API. In practice, the library needs to have the
   `get_keyword_names` or `getKeywordNames` method returning
   a list of keyword names that the library implements.

키워드 이름은 동적 API와 정확히 동일한 방법으로 얻을 수 있다. 실제로
라이브러리는 라이브러리가 구현한 키워드 이름의 목록을 반환하는
`get_keyword_names` 나 `getKeywordNames` 메소드를 가질 필요가 있다.

Running keywords
~~~~~~~~~~~~~~~~

..
   In the hybrid API, there is no `run_keyword` method for executing
   keywords. Instead, Robot Framework uses reflection to find methods
   implementing keywords, similarly as with the static API. A library
   using the hybrid API can either have those methods implemented
   directly or, more importantly, it can handle them dynamically.

하이브리드 API에서 키워드를 실행하기 위한 `run_keyword` 메소드는
존재하지 않는다. 대신에 Robot Framework 키워드를 구현한 메소드를
찾기위해 정적 API처럼 반영을 사용한다. 하이브리드 API를 사용하는
라이브러리는 직접적으로 구현된 이런 메소드를 가지거나 더 중요하게
동적으로 이것을 다룰 수 있다.

..
   In Python, it is easy to handle missing methods dynamically with the
   `__getattr__` method. This special method is probably familiar
   to most Python programmers and they can immediately understand the
   following example. Others may find it easier to consult `Python Reference
   Manual`__ first.

파이썬에서 `__getattr__` 메소드로 동적으로 누락된 메소드를 다루는 것은
쉽다. 이 특별한 방법은 아마 대부분의 파이썬 프로그래머에게 더 친숙하고
즉시 다음 예제를 이해할 수 있다. 다른 사람들은 먼저 `Python Reference
Manual`__ 을 참조해서 찾는 것이 더 쉬울지도 모른다.

__ http://docs.python.org/reference/datamodel.html#attribute-access

.. sourcecode:: python

   from somewhere import external_keyword

   class HybridExample:

       def get_keyword_names(self):
           return ['my_keyword', 'external_keyword']

       def my_keyword(self, arg):
           print "My Keyword called with '%s'" % arg

       def __getattr__(self, name):
           if name == 'external_keyword':
               return external_keyword
           raise AttributeError("Non-existing attribute '%s'" % name)

..
   Note that `__getattr__` does not execute the actual keyword like
   `run_keyword` does with the dynamic API. Instead, it only
   returns a callable object that is then executed by Robot Framework.

`__getattr__` 는 동적 API에서 `run_keyword` 처럼 실제 키워드를
실행하지 않는다. 대신에 단지 호출가능한 객체를 반환하고 Robot
Framework에 의해 실행된다.

..
   Another point to be noted is that Robot Framework uses the same names that
   are returned from `get_keyword_names` for finding the methods
   implementing them. Thus the names of the methods that are implemented in
   the class itself must be returned in the same format as they are
   defined. For example, the library above would not work correctly, if
   `get_keyword_names` returned `My Keyword` instead of
   `my_keyword`.

Robot Framework는 구현된 메소드를 찾기 위해 `get_keyword_names` 로
부터 반환된 리스트에서 동일한 이름을 사용한다. 그래서 클래스
그자체에서 구현된 메소드의 이름은 그들이 정의되어진 것과 동일한
형식으로 반환되어져야 한다. 예를 들어 위의 라이브러리는
`get_keyword_names` 가 `my_keyword` 대신에 `My Keyword` 를 반환하면
정확하게 동작하지 않는다.

..
   The hybrid API is not very useful with Java, because it is not
   possible to handle missing methods with it. Of course, it is possible
   to implement all the methods in the library class, but that brings few
   benefits compared to the static API.

하이브리드 API는 자바에서는 누락된 메소드를 다르는 것이 불가능하므로
별로 유용하지 않다. 물론 라이브러리 클래서 안헤 모든 메소드를 구현
할수 있지만 이렇게 하면 정적 API대비 장점이 적다.

Getting keyword arguments and documentation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When this API is used, Robot Framework uses reflection to find the
methods implementing keywords, similarly as with the static API. After
getting a reference to the method, it searches for arguments and
documentation from it, in the same way as when using the static
API. Thus there is no need for special methods for getting arguments
and documentation like there is with the dynamic API.

이 API를 사용하는 경우 Robot Framework는 정적 API와 비슷하게 구현하는
메소드를 찾기 위해 반영을 사용한다. 메소드에 대한 참조를 얻은 후에
정적 API를 사용할 때와 동일한 방법으로 전달인자와 문서를 찾는다.

Summary
~~~~~~~

..
   When implementing a test library in Python, the hybrid API has the same
   dynamic capabilities as the actual dynamic API. A great benefit with it is
   that there is no need to have special methods for getting keyword
   arguments and documentation. It is also often practical that the only real
   dynamic keywords need to be handled in `__getattr__` and others
   can be implemented directly in the main library class.

파이썬에서 테스트 라이브러리를 구현 할 경우 하이브리드 API는 실제 동적
API와 같은 동적 기능을 제공한다. 큰 장점은 키워드 인자오 문서를
취득하기 위한 특별한 방법을 가질 필요가 없다는 것이다. 이것은 단지
실제 동적 키워드는 `__getattr__` 로 취급할 필요가 있고, 나머지는 메인
라이브러리 클래스에서 직접 구현될 수 있어서 종종 실용적이다.


..
   Because of the clear benefits and equal capabilities, the hybrid API
   is in most cases a better alternative than the dynamic API when using
   Python. One notable exception is implementing a library as a proxy for
   an actual library implementation elsewhere, because then the actual
   keyword must be executed elsewhere and the proxy can only pass forward
   the keyword name and arguments.

명확한 장점과 동등한 성능 때문에 하이브리드 API는 대부분의 경우
파이썬을 사용할 경우 동적 API보다 나은 대안이다. 실제 키워드가 다른
곳에서 실행되어야 하고 프록시는 키워드 이름과 전달인자를 앞으로 통과
할 수 있기 때문에 하나의 주목할 만한 예외는 다른 곳에서 실제
라이브러리 구현을 위한 프록시 라이브러리를 구현하는 것이다.

..
   A good example of using the hybrid API is Robot Framework's own
   Telnet_ library.

하이브리드 API를 사용하는 좋은 예제는 Robot Framework 내의 Telnet_ 라이브러리이다.

Using Robot Framework's internal modules
----------------------------------------

..
   Test libraries implemented with Python can use Robot Framework's
   internal modules, for example, to get information about the executed
   tests and the settings that are used. This powerful mechanism to
   communicate with the framework should be used with care, though,
   because all Robot Framework's APIs are not meant to be used by
   externally and they might change radically between different framework
   versions.

파이썬으로 구현된 테스트 라이브러리는 예를 들어 실행된 테스트에 대한
정보를 얻거나 사용된 설정을 얻기 위해 Robot Framework의 내부 모듈을
사용할 수 있다. 프레임워크와 통신하기 위한 강력한 메카니즘은 주의 깊게
사용해야 한다. 왜냐하면 모든 Robot Framework API는 외부에서 사용할 수
있는 것이 아니며 서로 다른 버전의 프레임워크간 근본적으로 변경될 수
있기 때문이다.

Available APIs
~~~~~~~~~~~~~~

..
   Starting from Robot Framework 2.7, `API documentation`_ is hosted separately
   at the excellent `Read the Docs`_ service. If you are unsure how to use
   certain API or is using them forward compatible, please send a question
   to `mailing list`_.

Robot Framework 2.7 부터 `API documentation`_ 은 분리하여 `Read the
Docs`_ 서비스로 호스팅된다. 어떤 API에 대한 사용법이 확실하지 않거나
호환 사용법을 모르는 경우 `메일링 리스트 <mailing list_>`__ 에 질문을
보내기 바란다.


Using BuiltIn library
~~~~~~~~~~~~~~~~~~~~~

..
   The safest API to use are methods implementing keywords in the
   BuiltIn_ library. Changes to keywords are rare and they are always
   done so that old usage is first deprecated. One of the most useful
   methods is `replace_variables` which allows accessing currently
   available variables. The following example demonstrates how to get
   `${OUTPUT_DIR}` which is one of the many handy `automatic
   variables`_. It is also possible to set new variables from libraries
   using `set_test_variable`, `set_suite_variable` and
   `set_global_variable`.

사용하기에 가장 안전한 API는 BuiltIn_ 라이브러리에서 키워드를 구현한
메소드이다. 키워드에 대한 변경은 드물고 항상 행해진다. 그래서 오래된
사용이 가정 먼저 디프리케이트된다. 가장 유용한 메소드 중 하나는 현재
가용한 변수에 접근할 수 있는 `replace_variables` 이다. 다음 예제는
많은 편리한 `자동 변수 <automatic variables_>`__ 중의 하나인
`${OUTPUT_DIR}` 을 얻는 방법을 보여준다. 이것은 또한 라이브러리로부터
`set_test_variable`, `set_suite_variable`, `set_global_variable` 를
사용하여 새로운 변수를 설정할 수 있다.

.. sourcecode:: python

   import os.path
   from robot.libraries.BuiltIn import BuiltIn

   def do_something(argument):
       output = do_something_that_creates_a_lot_of_output(argument)
       outputdir = BuiltIn().replace_variables('${OUTPUTDIR}')
       path = os.path.join(outputdir, 'results.txt')
       f = open(path, 'w')
       f.write(output)
       f.close()
       print '*HTML* Output written to <a href="results.txt">results.txt</a>'

..
   The only catch with using methods from `BuiltIn` is that all
   `run_keyword` method variants must be handled specially.
   Methods that use `run_keyword` methods have to be registered
   as *run keywords* themselves using `register_run_keyword`
   method in `BuiltIn` module. This method's documentation explains
   why this needs to be done and obviously also how to do it.

단지 `BuiltIn` 의 메소드를 사용한 캐치는 모든 `run_keyword` 메소드
변종이 특별히 처리되어야 한다는 것이다. `run_keyword` 메소드를
사용하는 메소드는 `BuiltIn` 모듈의 `register_run_keyword` 를 사용하여
스스로 *run keywords* 로 등록되어야 한다. 이 메소드의 문서는 왜 이러한
작업을 수행해야 하는지와 수행하는 방법을 설명한다.


Extending existing test libraries
---------------------------------

..
   This section explains different approaches how to add new
   functionality to existing test libraries and how to use them in your
   own libraries otherwise.

이 섹션은 기존 테스트 라이브러리에 새로운 기능을 추가하는 방법과
자신의 라이브러리를 사용하는 방법에 대하여 다른 접근을 설명한다.

Modifying original source code
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   If you have access to the source code of the library you want to
   extend, you can naturally modify the source code directly. The biggest
   problem of this approach is that it can be hard for you to update the
   original library without affecting your changes. For users it may also
   be confusing to use a library that has different functionality than
   the original one. Repackaging the library may also be a big extra
   task.

확장하기 위한 라이브러리의 소스 코드에 접근할 수 있다면 소스 코드를
직접 수정하는 것이 자연스럽다. 이런 접근의 가장 큰 문제는 이런 변경이
영향을 미치지 않고 원본 라이브러리를 갱신하는 것이다. 사용자는 원래의
라이브러리보다 다른 기능을 가진 라이브러리의 사용을 혼란스러워 할 수
도 있다. 라이브러리를 재 포장하는 것은 큰 추가 작업이 될 수도 있다.

..
   This approach works extremely well if the enhancements are generic and
   you plan to submit them back to the original developers. If your
   changes are applied to the original library, they are included in the
   future releases and all the problems discussed above are mitigated. If
   changes are non-generic, or you for some other reason cannot submit
   them back, the approaches explained in the subsequent sections
   probably work better.

만약 확장이 일반적이고 원래 개발자에게 다시 제출하려면 이런 접근은
매우 잘 동작한다. 변경이 원본 라이브러리에 적용되는 경우 향후 릴리즈에
포함되며 위에서 논의된 모든 문제가 완화된다. 변화가 일반적인지 않거나
다른 이유로 다시 제출할 수 없다면 다음 섹션에서 설명되는 접근이 더 잘
작동한다.

Using inheritance
~~~~~~~~~~~~~~~~~

..
   Another straightforward way to extend an existing library is using
   inheritance. This is illustrated by the example below that adds new
   :name:`Title Should Start With` keyword to the SeleniumLibrary_. This
   example uses Python, but you can obviously extend an existing Java
   library in Java code the same way.

기존 라이브러리를 확장하는 또 다른 간단한 방법은 상속을 사용하는
것이다. 이것은 새로운 :name:`Title Should Start With` 키워드를
SeleniumLibrary_ 에 추가하는 아래 예제에서 설명한다. 이 예제는
파이썬을 사용하지만 동일한 방법으로 자바 코드에서 기존의 자바
라이브러리를 확장할 수 있다.

.. sourcecode:: python

   from SeleniumLibrary import SeleniumLibrary

   class ExtendedSeleniumLibrary(SeleniumLibrary):

       def title_should_start_with(self, expected):
           title = self.get_title()
           if not title.startswith(expected):
               raise AssertionError("Title '%s' did not start with '%s'"
                                    % (title, expected))

..
   A big difference with this approach compared to modifying the original
   library is that the new library has a different name than the
   original. A benefit is that you can easily tell that you are using a
   custom library, but a big problem is that you cannot easily use the
   new library with the original. First of all your new library will have
   same keywords as the original meaning that there is always
   conflict__. Another problem is that the libraries do not share their
   state.

원래 라이브러리를 수정하는 것에 비해서 큰 차이는 새로운 라이브러리가
원본과는 다른 이름을 가진다는 것이다. 이점은 쉽게 사용자 정의
라이브러리를 사용할 수 있지만, 원본과 함께 새로운 라이브러리를 쉽게
사용할 수 없는 것이 큰 문제이다. 가장 먼저 새로운 라이브러리는 항상
충돌__ 을 의미하는 원본과 동일한 키워드를 가지게 된다. 다른 문제는
라이브러리는 그들의 상태를 공유하지 않는다는 것이다.

..
   This approach works well when you start to use a new library and want
   to add custom enhancements to it from the beginning. Otherwise other
   mechanisms explained in this section are probably better.

새로운 라이브러리를 사용하기 시작하고 처음부터 사용자 정의 기능 향상을
추가할 때 이 방법은 잘 작동한다. 그렇지 않으면 이 섹션에 설명된 다른
메카니즘이 더 낫다.

__ `Handling keywords with same names`_

Using other libraries directly
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Because test libraries are technically just classes or modules, a
   simple way to use another library is importing it and using its
   methods. This approach works great when the methods are static and do
   not depend on the library state. This is illustrated by the earlier
   example that uses `Robot Framework's BuiltIn library`__.

테스트 라이브러리는 기술적으로 단지 클래스나 모듈이므로 다른
라이브러리를 사용하기 위한 간단한 방법은 임포팅해서 라이브러리의
메소드를 사용하는 것이다. 이 방법은 메소드가 정적이고 라이브러리의
상태에 의존하지 않을 때 아주 잘 동작한다. 이것은 `Robot Framework의
BuiltIn 라이브러리`__ 를 사용하는 이전의 예에 의해 설명된다.

..
   If the library has state, however, things may not work as you would
   hope.  The library instance you use in your library will not be the
   same as the framework uses, and thus changes done by executed keywords
   are not visible to your library. The next section explains how to get
   an access to the same library instance that the framework uses.

그러나 라이브러리가 상태를 가진다면, 희망하는 대로 동작하지 않을 지도
모른다. 라이브러리에서 사용하는 라이브러리 인스턴스는 프레임워크가
사용하는 것과 동일하지 않을 수도 있다. 그래서 실행된 키워드에 의해
행해진 변경은 라이브러리에서 볼 수 없다. 다음 섹션해서 프레임워크가
사용하는 동일한 라이브러리 인스턴스에 접근하는 방법에 대하여 설명한다.

__ `Using Robot Framework's internal modules`_

Getting active library instance from Robot Framework
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   BuiltIn_ keyword :name:`Get Library Instance` can be used to get the
   currently active library instance from the framework itself. The
   library instance returned by this keyword is the same as the framework
   itself uses, and thus there is no problem seeing the correct library
   state. Although this functionality is available as a keyword, it is
   typically used in test libraries directly by importing the :name:`BuiltIn`
   library class `as discussed earlier`__. The following example illustrates
   how to implement the same :name:`Title Should Start With` keyword as in
   the earlier example about `using inheritance`_.

BuiltIn_ 키워드 :name:`Get Library Instance` 는 프레임워크 자체에서
현재 실행중인 라이브러리 인스턴스를 가져오는데 사용할 수 있다. 이
키워드에 의해 반환된 라이브러리 인스턴스는 프레임워크 자체가 사용하는
것과 동일하며 정확한 라이브러리 상태를 보기위해서 아무 문제가 없다. 이
기능이 키워드로 사용할 수 있지만, 일반적으로 `이전에 논의된 바와
같이`__ :name:`BuiltIn` 라이브러리 클래스를 직접 임포팅해서 테스트
라이브러리를 사용된다. 다음 예는 이전의 `상속을 사용 <using
inheritance_>`__ 한 예와 동일한 :name:`Title Should Start With`
키워드를 구현하는 방법을 보여준다.

__ `Using Robot Framework's internal modules`_

.. sourcecode:: python

   from robot.libraries.BuiltIn import BuiltIn

   def title_should_start_with(expected):
       seleniumlib = BuiltIn().get_library_instance('SeleniumLibrary')
       title = seleniumlib.get_title()
       if not title.startswith(expected):
           raise AssertionError("Title '%s' did not start with '%s'"
                                % (title, expected))

..
   This approach is clearly better than importing the library directly
   and using it when the library has a state. The biggest benefit over
   inheritance is that you can use the original library normally and use
   the new library in addition to it when needed. That is demonstrated in
   the example below where the code from the previous examples is
   expected to be available in a new library :name:`SeLibExtensions`.

이런 접근은 직접 라이브러리를 임포팅하여 라이브러리 상태가 있을 때
그것을 사용하는 것보다 명백히 낫다. 상속을 통한 가장 큰 이점은
일반적으로 원래의 라이브러리를 사용하고 필요할때 새로운 라이브러리를
사용할 수 있다는 것이다. 아래 예제는 새로운 라이브러리
:name:`SeLibExtensions` 를 이전의 예에서 가져와서 사용하는 것을
보여준다.

.. sourcecode:: robotframework

   *** Settings ***
   Library    SeleniumLibrary
   Library    SeLibExtensions

   *** Test Cases ***
   Example
       Open Browser    http://example      # SeleniumLibrary
       Title Should Start With    Example  # SeLibExtensions

Libraries using dynamic or hybrid API
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Test libraries that use the dynamic__ or `hybrid library API`_ often
   have their own systems how to extend them. With these libraries you
   need to ask guidance from the library developers or consult the
   library documentation or source code.

Dynamic__ 또는 `hybrid library API`_ 를 사용하는 테스트 라이브러리는
종종 확장하는 방법에 대한 자신의 시스템을 가진다. 이 라이브러리를
사용하면 라이브러리 개발자로부터 아내를 요청하거나 라이브러리 문서
또는 소스코드를 참조할 필요가 있다.

__ `dynamic library API`_
