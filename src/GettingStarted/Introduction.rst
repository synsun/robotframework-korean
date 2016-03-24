Introduction
============

..
   Robot Framework is a Python-based, extensible keyword-driven test
   automation framework for end-to-end acceptance testing and
   acceptance-test-driven development (ATDD). It can be used for testing
   distributed, heterogeneous applications, where verification requires
   touching several technologies and interfaces.

Robot Framework는 종단간(end-to-end) 인수 테스팅(acceptance testing)과
인수 테스트 주도 개발(acceptance-testing-driven development, ATTD)위한
파이썬에 기초한 확장가능한 키워드 주도 테스트 자동화
프레임워크(keyword-driven test automation framework)이다. 여러가지
기술과 인터페이스에 대하여 검증(verification)을 해야 하는
분산된(distributed), 이종(heterogeneous) 애플리케이션 테스팅을 위해서
사용할 수 있다.

.. contents::
   :depth: 2
   :local:

Why Robot Framework?
--------------------

..
   - Enables easy-to-use tabular syntax for `creating test cases`_ in a uniform
     way.

- 균일한(uniform) 방법으로 `test cases를 만들기 <creating test
  cases_>`__ 위한 사용하기 쉬운 표 문법(easy-to-use tabular syntax)을
  사용한다.
  
..
   - Provides ability to create reusable `higher-level keywords`_ from the
     existing keywords.

- 기존 키워드로부터 재사용할 수 있는 `고수준 키워드 <higher-level
  keywords_>`__ (higher-level keywords)생성할 수 있는 능력을 제공한다.
     
..
   - Provides easy-to-read result reports_ and logs_ in HTML format.

- HTML 형식의 쉽게 읽을 수 있는 결과 reports_ 와 logs_ 를 제공한다.

..
   - Is platform and application independent.

- 플랫폼 및 애플리케이션에 독립적이다.
     
..
   - Provides a simple `library API`_ for creating customized test libraries
     which can be implemented natively with either Python or Java.

- 파이썬이나 자바로 구현될 수 있는 사용자 정의 테스트 라이브러리를
  생성하기 위한 간단한 `library API`_ 를 제공한다.
     
..
   - Provides a `command line interface`__ and XML based `output files`_  for
     integration into existing build infrastructure (continuous integration
     systems).

- 기존 빌드 인프라(지속적 통합 시스템)에 통합을 위한 `명령행
  인터페이스`__ 와 XML에 기초한 `output files`_ 을 제공한다.
     
..
   - Provides support for Selenium for web testing, Java GUI testing, running
     processes, Telnet, SSH, and so on.

- Web 테스팅을 위한 Selenium, Java GUI 테스팅, 실행중인 프로세스,
  Telnet, SSH 기타등등에 대한 지원을 제공한다.
     
..
   - Supports creating `data-driven test cases`__.

- `data-driven test cases`__ 작성을 지원한다.
     
..
   - Has built-in support for variables_, practical particularly for testing in
     different environments.

- 특히 실제적으로 서로 다른 환경에서 테스팅을 위한 `변수
  <variables_>`__ 에 대하여 기본(built-in) 지원을 한다.
     
..
   - Provides tagging__ to categorize and `select test cases`__ to be executed.

- 실행될 test cases를 분류하고 선택__ 하기 위하여 태그__ 를 제공한다.
     
..
   - Enables easy integration with source control: `test suites`_ are just files
     and directories that can be versioned with the production code.

- 소스제어와 쉽게 통합 할 수 있다: `test suites`_ 은 프로덕션 코드와
  함께 버전관리가 될 수 있는 단순한 파일과 디렉토리다.
     
..
   - Provides `test-case`__ and `test-suite`__ -level setup and teardown.

- `test-case`__ 와 `test-suite`__ 레벨 setup과 teardown을 제공한다.
     
..
   - The modular architecture supports creating tests even for applications with
     several diverse interfaces.

- 모듈형 아키텍처는 여러 다양한 인터페이스를 가지는 애플리케이션까지도
  테스트를 만들수 있도록 지원한다.
     
__ `Executing test cases`_
__ `Data-driven style`_
__ `Selecting test cases`_
__ `Tagging test cases`_
__ `Test setup and teardown`_
__ `Suite setup and teardown`_


High-level architecture
-----------------------

..
   Robot Framework is a generic, application and technology independent
   framework. It has a highly modular architecture illustrated in the
   diagram below.

Robot Framework는 일반적인 애플리케이션 및 기술 독립적인
프레임워크이고, 아래 그림과 같이 고도의 모듈형 아키텍처를 가진다.

.. figure:: src/GettingStarted/architecture.png

   Robot Framework architecture

..
   The `test data`_ is in simple, easy-to-edit tabular format. When
   Robot Framework is started, it processes the test data, `executes test
   cases`__ and generates logs and reports. The core framework does not
   know anything about the target under test, and the interaction with it
   is handled by `test libraries`__. Libraries can either use application
   interfaces directly or use lower level test tools as drivers.

`Test data`_ 는 간단하고 편집하기 쉬운 표 형식이다. Robot Framework가
시작될 때 테스트 데이터를 처리하여 `test cases를 실행하고`__ logs와
reports를 생성한다. 코어 프레임워크는 시험 대상에 대해 아무것도 모르고
시험 대상과 상호작용은 `테스트 라이브러리`__ 에 의해 처리된다.
라이브러리는 애플리케이션 인터페이스를 직접 사용하거나 드라이버와 같은
저 수준 테스트 툴을 사용할 수 있다.
   
__ `Executing test cases`_
__ `Creating test libraries`_


Screenshots
-----------

..
   Following screenshots show examples of the `test data`_ and created
   reports_ and logs_.

다음의 스크린샷은 `test data`_ 와 생성된 reports_ 및 logs_ 예제를
보여준다.

.. figure:: src/GettingStarted/testdata_screenshots.png

   Test case files

.. figure:: src/GettingStarted/screenshots.png

   Reports and logs


Getting more information
------------------------

Project pages
~~~~~~~~~~~~~

..
   The number one place to find more information about Robot Framework
   and the rich ecosystem around it is http://robotframework.org.
   Robot Framework itself is hosted on GitHub__.

Robot Framework에 대한 더 자세한 정보와 주변의 풍부한 생태계는
http://robotframework.org 이다. Robot Framework 자체는 GitHub__ 에서
호스팅한다.
   
__ https://github.com/robotframework/robotframework

Mailing lists
~~~~~~~~~~~~~

There are several Robot Framework mailing lists where to ask and
search for more information. The mailing list archives are open for
everyone (including the search engines) and everyone can also join
these lists freely. Only list members can send mails, though, and to
prevent spam new users are moderated which means that it might take a
little time before your first message goes through.  Do not be afraid
to send question to mailing lists but remember `How To Ask Questions
The Smart Way`__.

robotframework-users__
   General discussion about all Robot Framework related
   issues. Questions and problems can be sent to this list. Used also
   for information sharing for all users.

robotframework-announce__
    An announcements-only mailing list where only moderators can send
    messages. All announcements are sent also to the
    robotframework-users mailing list so there is no need to join both
    lists.

robotframework-devel__
   Discussion about Robot Framework development.

__ http://www.catb.org/~esr/faqs/smart-questions.html
__ http://groups.google.com/group/robotframework-users
__ http://groups.google.com/group/robotframework-announce
__ http://groups.google.com/group/robotframework-devel
