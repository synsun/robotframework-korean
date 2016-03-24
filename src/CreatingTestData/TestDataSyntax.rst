Test data syntax
================

..
   This section covers Robot Framework's overall test data
   syntax. The following sections will explain how to actually create test
   cases, test suites and so on.

본 섹션에서는 Robot Framework의 전반적인 test data syntax을 다룬다.
이어지는 섹션에서는 실제로 어떻게 test suites 와 test cases 등을
생성하는지 설명한다.

.. contents::
   :depth: 2
   :local:

Files and directories
---------------------

..
   The hierarchical structure for arranging test cases is built as follows:

   - Test cases are created in `test case files`_.
   - A test case file automatically creates a `test suite`_ containing
     the test cases in that file.
   - A directory containing test case files forms a higher-level test
     suite. Such a `test suite directory`_ has suites created from test
     case files as its sub test suites.
   - A test suite directory can also contain other test suite directories,
     and this hierarchical structure can be as deeply nested as needed.
   - Test suite directories can have a special `initialization file`_.

테스트 케이스를 배열하는 계층적 구조는 다음과 같이 형성되어있다.

- Test cases 는 `test case files`_ 안에 생성된다.
- Test case 파일은 자동적으로 test cases를 포함하는 `test suite`_ 를
  생성한다.
- Test case 파일을 포함하는 디렉토리는 상위 레벨의 test suite을
  구성한다. 그런 `test suite directory`_ 는 test case 파일로부터
  생성된 suites를 sub test suites로 가진다.
- test suite 디렉토리는 다른 test suite 디렉토리도 포함할 수 있고,
  이러한 계층적 구조는 필요한만큼 반복하여 내포할 수 있다.
- Test suite 디렉토리는 특별한 `initialization file`_ 을 포함한다.

..
   In addition to this, there are:

   - `Test libraries`_ containing the lowest-level keywords.
   - `Resource files`_ with variables_ and higher-level `user keywords`_.
   - `Variable files`_ to provide more flexible ways to create variables
     than resource files.

추가로 :

- `Test libraries`_ 는 lowest-level 키워드를 포함한다.
- `Resource files`_ 은 variables_ 와 higher-level `user keywords`_ 를
  포함한다.
- Resource files보다 `Variable files`_ 가 변수를 생성하는 데 유연한
  방법을 제공한다.

Supported file formats
----------------------

..
   Robot Framework test data is defined in tabular format, using either
   hypertext markup language (HTML), tab-separated values (TSV),
   plain text, or reStructuredText (reST) formats. The details of these
   formats, as well as the main benefits and problems with them, are explained
   in the subsequent sections. Which format to use depends on the context,
   but the plain text format is recommended if there are no special needs.

Robot Framework 테스트 데이터는 HTML, TSV(tab-separated values), plain
text, 또는 reST(reStructuredText) 포맷을 사용하여 표 형식으로
정의한다. 이 포맷에 대한 자세한 내용과 장단점에 대해서는 다음 섹션에서
다룬다. 어떤 형식을 사용할지는 문맥에 따라 다르겠지만, 특별한 요구가
없다면 plain text 형식을 추천한다.

..
   Robot Framework selects a parser for the test data based on the file extension.
   The extension is case-insensitive, and the recognized extensions are
   :file:`.html`, :file:`.htm` and :file:`.xhtml` for HTML, :file:`.tsv`
   for TSV, :file:`.txt` and special :file:`.robot` for plain text, and
   :file:`.rst` and :file:`.rest` for reStructuredText.

Robot Framework는 파일 확장자에 따라 테스트 데이터 parser를 선택한다.
확장자는 대소문자를 구분하지 않고, 공인된 확장자는 :file:`.html`,
:file:`.htm`, :file:`.xhtml`, :file:`.tsv`, :file:`.txt`,
:file:`.robot`, :file:`.rst`, :file:`.rest` 가 있다.

..
   Different `test data templates`_ are available for HTML and TSV
   formats to make it easier to get started writing tests.

테스트 작성을 쉽게 시작할 수 있도록 HTML 과 TSV 포맷을 위한 별도의
`test data templates`_ 을 제공한다.

..
   .. note:: The special :file:`.robot` extension with plain text files is
	     supported starting from Robot Framework 2.7.6.

.. note:: Plain text 파일의 .robot 확장자는 Robot Framework 2.7.6.부터
          지원된다.

HTML format
~~~~~~~~~~~

..
   HTML files support formatting and free text around tables. This makes it
   possible to add additional information into test case files and allows creating
   test case files that look like formal test specifications. The main problem
   with HTML format is that editing these files using normal text editors is not
   that easy. Another problem is that HTML does not work as well with version
   control systems because the diffs resulting from changes contain HTML syntax
   in addition to changes to the actual test data.

HTML 파일은 표 주위에 서식과 문자를 자유롭게 사용할 수 있다. 추가적인
정보를 테스트 케이스 파일에 추가 할 수 있고, 테스트 케이스 파일을
형식적인 테스트 설계명세서 처럼 보이게 할 수 있다. HTML 형식의 가장 큰
문제점은 텍스트 에디터를 사용하여 파일을 수정하는 것이 쉽지 않다는
것이다. HTML은 테스트 데이터 변경 내용 뿐만 아니라 HTML 문법을
포함하기 때문에 버전 컨트롤 시스템을 이용하기 쉽지 않다는 문제점도
있다.

..
   In HTML files, the test data is defined in separate tables (see the example below).
   Robot Framework recognizes these `test data tables`_ based on the text in their first cell.
   Everything outside recognized tables is ignored.

HTML 파일에서, 테스트 데이터는 별도의 표에 정의한다. (아래 예제 참고)
Robot Framework는 첫번째 셀에 적혀진 text를 기반으로 `test data
tables`_ 를 인식한다. 그 밖의 외부표는 모두 무시한다.

.. table:: Using the HTML format
   :class: example

   ============  ================  =======  =======
      Setting          Value        Value    Value
   ============  ================  =======  =======
   Library       OperatingSystem
   \
   ============  ================  =======  =======

.. table::
   :class: example

   ============  ================  =======  =======
     Variable        Value          Value    Value
   ============  ================  =======  =======
   ${MESSAGE}    Hello, world!
   \
   ============  ================  =======  =======

.. table::
   :class: example

   ============  ===================  ============  =============
    Test Case           Action          Argument      Argument
   ============  ===================  ============  =============
   My Test       [Documentation]      Example test
   \             Log                  ${MESSAGE}
   \             My Keyword           /tmp
   \
   Another Test  Should Be Equal      ${MESSAGE}    Hello, world!
   ============  ===================  ============  =============

.. table::
   :class: example

   ============  ======================  ============  ==========
     Keyword            Action             Argument     Argument
   ============  ======================  ============  ==========
   My Keyword    [Arguments]             ${path}
   \             Directory Should Exist  ${path}
   ============  ======================  ============  ==========

Editing test data
'''''''''''''''''

..
   Test data in HTML files can be edited with whichever editor you
   prefer, but a graphic editor, where you can actually see the tables,
   is recommended. RIDE_ can read and write HTML files, but unfortunately
   it loses all HTML formatting and also possible data outside test case
   tables.

HTML 파일에 있는 테스트 데이터는 모든 에디터로 수정할 수 있지만, 표를
볼 수 있는 그래픽 에디터를 추천한다. RIDE_ 는 HTML 파일을 읽고 쓸 수
있지만, HTML 형식 전부와 테스트 케이스 표 이외의 것을 삭제한다.

Encoding and entity references
''''''''''''''''''''''''''''''

..
   HTML entity references (for example, `&auml;`) are
   supported. Additionally, any encoding can be used, assuming that it is
   specified in the data file. Normal HTML files must use the META
   element as in the example below::

HTML 엔터티 참조(예, `&auml;`)를 지원한다. 추가적으로 임의의 인코딩이
사용가능하고, 데이터 파일 내에 인코딩을 명시해야 한다. 일반적인 HTML
파일은 META 요소를 아래 예제와 같이 사용한다::

  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

..
   XHTML files should use the XML preamble as in this example::

XHTML 파일들은 XML 전문을 아래 예제처럼 사용해야 한다::

  <?xml version="1.0" encoding="Big5"?>

..
   If no encoding is specified, Robot Framework uses ISO-8859-1 by default.

만일 어떤 인코딩도 명시되지 않았다면, Robot Framework는 ISO-8859-1을
기본값으로 사용한다.

TSV format
~~~~~~~~~~

..
   TSV files can be edited in spreadsheet programs and, because the syntax is
   so simple, they are easy to generate programmatically. They are also pretty
   easy to edit using normal text editors and they work well in version control,
   but the `plain text format`_ is even better suited for these purposes.

TSV 파일은 스프레드시트 프로그램으로 수정할 수 있다. 구문이 매우
간단하기 때문에 프로그래밍으로 생성하기 쉽다. 또한 일반 텍스트
에디터를 이용해서 수정하는 것도 쉽고, 버전 컨트롤 시스템과 잘
동작한다. 하지만 버전 컨트롤 시스템에는 `plain text format`_ 가 더
적합하다.

..
   The TSV format can be used in Robot Framework's test data for all the
   same purposes as HTML. In a TSV file, all the data is in one large
   table. `Test data tables`_ are recognized from one or more asterisks
   (`*`), followed by a normal table name and an optional closing
   asterisks.  Everything before the first recognized table is ignored
   similarly as data outside tables in HTML data.

TSV 형식은 Robot Framework의 테스트 데이터에서 HTML과 같은 목적으로
사용될 수 있다. TSV 파일에서, 모든 데이터는 하나의 표에 담긴다. `Test
data tables`_ 은 하나 이상의 별표(`*`)와 표 이름 그리고 닫는
별표(선택사항)로 인식한다. 처음 인식한 테이블 이전의 모든것은 HTML
데이터의 표 밖의 데이터 처럼 무시한다.

.. table:: Using the TSV format
   :class: tsv-example

   ============  =======================  =============  =============
   \*Setting*    \*Value*                 \*Value*       \*Value*
   Library       OperatingSystem
   \
   \
   \*Variable*   \*Value*                 \*Value*       \*Value*
   ${MESSAGE}    Hello, world!
   \
   \
   \*Test Case*  \*Action*                \*Argument*    \*Argument*
   My Test       [Documentation]          Example test
   \             Log                      ${MESSAGE}
   \             My Keyword               /tmp
   \
   Another Test  Should Be Equal          ${MESSAGE}     Hello, world!
   \
   \
   \*Keyword*    \*Action*                \*Argument*    \*Argument*
   My Keyword    [Arguments]              ${path}
   \             Directory Should Exist   ${path}
   ============  =======================  =============  =============

Editing test data
'''''''''''''''''

..
   You can create and edit TSV files in any spreadsheet program, such as
   Microsoft Excel. Select the tab-separated format when you save the
   file and remember to set the file extension to :file:`.tsv`. It is
   also a good idea to turn all automatic corrections off and configure
   the tool to treat all values in the file as plain text.

TSV 파일은 어떤 스프레드시트(예, MS excel) 프로그램을 이용하든
생성하고 수정할 수 있다. 파일을 저장할 때 탭이 분리된 형식을 선택하고
파일의 확장자를 :file:`.tsv` 로 한다. 자동수정 기능을 끄고 파일의 모든
값을 plain text로 취급하도록 환경을 설정한다.

..
   TSV files are relatively easy to edit with any text editor,
   especially if the editor supports visually separating tabs from
   spaces. The TSV format is also supported by RIDE_.

TSV 파일은 상대적으로 텍스트 에디터로 수정하기 쉽다. 특히 에디터가
시각적으로 공백과 탭을 구분하여 보여준다면 더욱 쉽다. TSV 형식 또한
RIDE_ 에서 지원한다.

..
   Robot Framework parses TSV data by first splitting all the content
   into rows and then rows into cells on the basis of the tabular
   characters. Spreadsheet programs sometimes surround cells with quotes
   (for example, `"my value"`) and Robot Framework removes
   them. Possible quotes inside the data are doubled (for example,
   `"my ""quoted"" value"`) and also this is handled correctly.  If
   you are using a spreadsheet program to create TSV data, you should not
   need to pay attention to this, but if you create data
   programmatically, you have to follow the same quoting conventions as
   spreadsheets.

Robot Framework는 TSV 데이터를 분석한다. 먼저 모든 내용을 줄로
분리하고, 줄들을 tabular characters를 기준으로 셀로 나눈다.
스프레드시트 프로그램은 때때로 셀을 따옴표로 둘러싸는데(예를들어 '"my
value"') Robot Framework는 그것을 제거한다. 데이터 안에서 따옴표는
쌍따옴표만 가능하다.(예를들어, '"my""quoted""value") 만일 스프레드시트
프로그램에서 TSV 데이터를 수정하려고 하는 경우, 이것들을 신경쓸 필요는
없다. 하지만 프로그래밍으로 데이터를 생성한다면 스프레드시트의 따옴표
표기방식을 따라야 한다.

Encoding
''''''''

..
   TSV files are always expected to use UTF-8 encoding. Because ASCII is
   a subset of UTF-8, plain ASCII is naturally supported too.

TSV 파일은 항상 UTF-8 인코딩을 사용한다. ASCII는 UTF-8에 포함되기
때문에, plain ASCII 역시 지원한다.

Plain text format
~~~~~~~~~~~~~~~~~

..
   The plain texts format is very easy to edit using any text editor and
   they also work very well in version control. Because of these benefits
   it has became the most used data format with Robot Framework.

Plain text 형식은 텍스트 에디터로 수정하기 쉽고 버전 컨트롤 시스템과
잘 동작한다. 이런 장점 때문에 Robot Framework에서 가장 많이 사용한다.

..
   The plain text format is technically otherwise similar to the `TSV
   format`_ but the separator between the cells is different. The TSV
   format uses tabs, but in the plain text format you can use either two
   or more spaces or a pipe character surrounded with spaces (:codesc:`\ |\ `).

Plain text 형식은 기술적으로 `TSV format`_ 와 비슷한면도 있지만 셀
구분자가 다르다. TSV 형식은 탭을 사용하지만, plain text 형식에서는 두
칸 이상의 공백이나 양쪽에 공백을 가지는 파이프 문자를 사용한다.
(:codesc:`\_|\_`, `_` 는 공백을 표시한다.)

..
   The `test data tables`_ must have one or more asterisk before their
   names similarly as in the TSV format. Otherwise asterisks and possible
   spaces in the table header are ignored so, for example, `***
   Settings ***` and `*Settings` work the same way. Also similarly
   as in the TSV format, everything before the first table is ignored.

`test data tables`_ 은 TSV 형식처럼 이름 앞에 하나 이상의 별표를
가진다. 표 헤더에서 하나  이상의 별표와 공백은 무시된다. 예를들어 `***
Settings ***` 와 `*Settings` 는 같은 방식으로 인식된다. TSV 형식과
비슷하게, 첫번째 표 이전의 모든 것은 무시한다.

..
   In plain text files tabs are automatically converted to two
   spaces. This allows using a single tab as a separator similarly as in
   the TSV format. Notice, however, that in the plain text format
   multiple tabs are considered to be a single separator whereas in the
   TSV format every tab would be a separator.

Plain text 파일에서 탭은 자동으로 공백 두 칸으로 변환된다. 이러한
변환때문에 TSV 형식처럼 탭 하나를 구분자로 사용할 수 있다. Plain text
형식에서 여러개의 탭은 하나의 구분자로 한다. 반면에 TSV형식에서는 모든
탭이 다 구분자이다.

Space separated format
''''''''''''''''''''''

..
   The number of spaces used as separator can vary, as long as there are
   at least two spaces, and it is thus possible to align the data nicely.
   This is a clear benefit over editing the TSV format in a text editor
   because with TSV the alignment cannot be controlled.

구분자로 사용하는 공백의 수는 다양할 수 있다. 적어도 두 개 이상의
공백이 있으면 된다. 그래서 이점을 이용하면 데이터를 이쁘게 정렬할 수
있다. 이것은 정렬이 불가능한 TSV 형식을 텍스트 에디터에서 수정할 때
보다 명확하다.

.. sourcecode:: robotframework

   *** Settings ***
   Library       OperatingSystem

   *** Variables ***
   ${MESSAGE}    Hello, world!

   *** Test Cases ***
   My Test
       [Documentation]    Example test
       Log    ${MESSAGE}
       My Keyword    /tmp

   Another Test
       Should Be Equal    ${MESSAGE}    Hello, world!

   *** Keywords ***
   My Keyword
       [Arguments]    ${path}
       Directory Should Exist    ${path}

..
   Because space is used as separator, all empty cells must be escaped__
   with `${EMPTY}` variable or a single backslash. Otherwise
   `handling whitespace`_ is not different than in other test data
   because leading, trailing, and consecutive spaces must always be
   escaped.

공백이 구분자로 사용되기 때문에, 모든 빈 셀은 `${EMPTY}` 변수나 하나의
백슬래시로 escaped__ 되어야 한다. 그렇지 않으면 앞, 뒤, 연속된 공백은
언제나 무시 되기 때문에 `handling whitespace`_ 는 다른 테스트
데이터들과 다를게 없다.

__ Escaping_

..
   .. tip:: It is recommend to use four spaces between keywords and arguments.

.. tip:: 키워드와 전달인자 사이에 4칸의 공백을 사용하는 것을 추천한다.

.. _pipe separated format:

Pipe and space separated format
'''''''''''''''''''''''''''''''

..
   The biggest problem of the space delimited format is that visually
   separating keywords form arguments can be tricky. This is a problem
   especially if keywords take a lot of arguments and/or arguments
   contain spaces. In such cases the pipe and space delimited variant can
   work better because it makes the cell boundary more visible.

공백으로 구분하는 형식의 가장 큰 문제점은 시각적으로 키워드와
전달인자를 구분하는 것이 까다롭다는 것이다. 특히 키워드가 많은
전달인자를 가지거나 전달인자에 공백이 있는 경우 더 문제가 된다. 이런
경우 파이프와 공백으로 구분하는 것이 셀 경계를 더 잘 보여주기 때문에
더 잘 동작한다.

.. sourcecode:: robotframework

   | *Setting*  |     *Value*     |
   | Library    | OperatingSystem |

   | *Variable* |     *Value*     |
   | ${MESSAGE} | Hello, world!   |

   | *Test Case*  | *Action*        | *Argument*   |
   | My Test      | [Documentation] | Example test |
   |              | Log             | ${MESSAGE}   |
   |              | My Keyword      | /tmp         |
   | Another Test | Should Be Equal | ${MESSAGE}   | Hello, world!

   | *Keyword*  |
   | My Keyword | [Arguments] | ${path}
   |            | Directory Should Exist | ${path}

..
   A plain text file can contain test data in both space-only and
   space-and-pipe separated formats, but a single line must always use
   the same separator. Pipe and space separated lines are recognized by
   the mandatory leading pipe, but the pipe at the end of the line is
   optional. There must always be at least one space on both sides of the
   pipe (except at the beginning and end) but there is no need to align
   the pipes other than if it makes the data more clear.

Plain text 파일은 공백만 있거나 공백과 파이프를 같이 사용 형식의
테스트 데이터를 포함할 수 있다. 하지만 한 줄에서는 항상 동일한
구분자를 사용해야 한다. 파이프와 공백으로 구분된 줄은 먼저 나오는
파이프로 구분한다. 하지만 줄의 마지막에 있는 파이프는 선택사항이다.
처음과 끝을 제외하고, 적어도 파이프 양쪽에 공백 한칸이 있어야 한다.
하지만 데이터를 더 명확하게 하는게 아니라면 파이프를 정렬할 필요는
없다.

..
   There is no need to escape empty cells (other than the `trailing empty
   cells`__) when using the pipe and space separated format. The only
   thing to take into account is that possible pipes surrounded by spaces
   in the actual test data must be escaped with a backslash:

파이프와 공백으로 구분하는형식을 사용할 때는 빈 셀(`trailing empty
cells`__ 과 비교)을 이스케이프 할 필요가 없다. 유일하게 고려해야 할
것은, 실제 테스트 데이터에서 공백으로 둘러쌓인 파이프는 반드시
백슬래시를 사용하여 이스케이프 시켜야 한다:

.. sourcecode:: robotframework

   | *** Test Cases *** |                 |                 |                      |
   | Escaping Pipe      | ${file count} = | Execute Command | ls -1 *.txt \| wc -l |
   |                    | Should Be Equal | ${file count}   | 42                   |

__ Escaping_

Editing and encoding
''''''''''''''''''''

..
   One of the biggest benefit of the plain text format over HTML and TSV
   is that editing it using normal text editors is very easy. Many editors
   and IDEs (at least Eclipse, Emacs, Vim, and TextMate) also have plugins that
   support syntax highlighting Robot Framework test data and may also provide
   other features such as keyword completion. The plain text format is also
   supported by RIDE_.

HTML과 TSV에 비해 Plain text 형식의 가장 큰 장점은 일반 텍스트
에디터로 매우 쉽게 수정할 수 있다는 것이다. 많은 에디터와 IDE는
(Eclipse, Emacs, Vim, TextMate 등) 문법 하이라이팅, 키워드 자동완성
등의 기능을 지원하는 플러그인이 있다. 물론 RIDE_ 는 Plain text 형식도
지원한다.

..
   Similarly as with the TSV test data, plain text files are always expected
   to use UTF-8 encoding. As a consequence also ASCII files are supported.

TSV 테스트 데이터와 비슷하게, plain text 파일은 항상 UTF-8 인코딩을
기본으로 하기때문에 결과적으로 ASCII 파일도 지원한다.

Recognized extensions
'''''''''''''''''''''

..
   Starting from Robot Framework 2.7.6, it is possible to save plain text
   test data files using a special :file:`.robot` extension in addition to
   the normal :file:`.txt` extension. The new extension makes it easier to
   distinguish test data files from other plain text files.

Robot Framework 2.7.6부터, plain text 테스트 데이터 파일을 특별히
:file:`.robot` 확장자와 :file:`.txt` 를 이용해서 저장할 수 있다.
새로운 확장자(:file:`.robot`)는 테스트 데이터 파일을 다른 plain text
파일과 구분하기 쉽게 해준다.

reStructuredText format
~~~~~~~~~~~~~~~~~~~~~~~

..
   reStructuredText_ (reST) is an easy-to-read plain text markup syntax that
   is commonly used for documentation of Python projects (including
   Python itself, as well as this User Guide). reST documents are most
   often compiled to HTML, but also other output formats are supported.

reStructuredText_ (reST)는 파이썬 프로젝트 문서와 파이썬 사용자 가이드
문서에 주로 쓰이는 읽기 쉬운 plain text markup syntax 이다. reST
문서는 대개 HTML로 컴파일 되지만 다른 포맷도 지원한다.

..
   Using reST with Robot Framework allows you to mix richly formatted documents
   and test data in a concise text format that is easy to work with
   using simple text editors, diff tools, and source control systems.
   In practice it combines many of the benefits of plain text and HTML formats.

Robot Framework에서 reST를 사용하면 좀더 풍부하게 형식화된 문서를
작성할 수 있다. 그리고 간단한 텍스트 에디터, diff 툴, 소스 컨트롤
시스템과 쉽게 연동할 수 있다. 실제로 reST는 plain text와 HTML 형식의
많은 장점을 결합한다.

..
   When using reST files with Robot Framework, there are two ways to define the
   test data. Either you can use `code blocks`__ and define test cases in them
   using the `plain text format`_ or alternatively you can use tables__ exactly
   like you would with the `HTML format`_.

..
   .. note:: Using reST files with Robot Framework requires the Python docutils_
	     module to be installed.

Robot Framework에서 reST 파일을 사용할 때, 테스트 데이터를 두가지
방법으로 정의 가능하다. `code blocks`__ 을 사용하고 그 안에 `plain text
format`_ 으로 작성하거나 `HTML format`_ 의 tables__ 처럼 작성할 수 있다.

.. note:: Robot Framework에서 reST 파일을 이용할 때 파이썬 docutils_
          모듈이 설치되어 있어야 한다.

__ `Using code blocks`_
__ `Using tables`_

Using code blocks
'''''''''''''''''

..
   reStructuredText documents can contain code examples in so called code blocks.
   When these documents are compiled into HTML or other formats, the code blocks
   are syntax highlighted using Pygments_. In standard reST code blocks are
   started using the `code` directive, but Sphinx_ uses `code-block`
   or `sourcecode` instead. The name of the programming language in
   the code block is given as an argument to the directive. For example, following
   code blocks contain Python and Robot Framework examples, respectively:

reStructuredText 문서는 코드 블록내에 코드 예제를 포함할 수 있다. 이
문서가 HTML 혹은 다른 형식으로 컴파일될 때, 코드 블록은 Pygments_ 을
사용해서 문법 강조(하이라이팅)한다. 표준 reST 코드 블록은 `code`
지시자를 사용하지만, Sphinx_ 는 `code-block` 혹은 `sourcecode` 를
사용한다. 코드 블록에서 사용하는 프로그래밍 언어 이름은 지시자의
전달인자로 주어진다. 다음 예는 코드블록이 파이썬과 Robot Framework
예제를 포함하는 것을 보여준다:

.. sourcecode:: rest

    .. code:: python

       def example_keyword():
           print 'Hello, world!'

    .. code:: robotframework

       *** Test Cases ***
       Example Test
           Example Keyword

..
   When Robot Framework parses reStructuredText files, it first searches for
   possible `code`, `code-block` or `sourcecode` blocks
   containing Robot Framework test data. If such code blocks are found, data
   they contain is written into an in-memory file and executed. All data outside
   the code blocks is ignored.

Robot Framework가 reStructuredText 파일을 파싱 할 때, 먼저 Robot
Framework 테스트 데이터를 포함하는 `code`, `code-block` 혹은
`sourcecode` 블록을 찾는다. 만약 그런 코드 블록이 발견되면, 코드
블록내의 데이터는 in-memory 파일에 쓰여지고 수행된다. 물론, 코드 블록
밖의 모든 데이터는 무시된다.

..
   The test data in the code blocks must be defined using the `plain text format`_.
   As the example below illustrates, both space and pipe separated variants are
   supported:

코드 블록의 테스트 데이터는 `plain text format`_ 으로 기술한다. 아래의
예시처럼 공백 및 파이프로 분리하는 두 형식 모두 지원한다:

.. sourcecode:: rest

    Example
    -------

    This text is outside code blocks and thus ignored.

    .. code:: robotframework

       *** Settings ***
       Library       OperatingSystem

       *** Variables ***
       ${MESSAGE}    Hello, world!

       *** Test Cases ***
       My Test
           [Documentation]    Example test
           Log    ${MESSAGE}
           My Keyword    /tmp

       Another Test
           Should Be Equal    ${MESSAGE}    Hello, world!

    이 텍스트는 코드 블록 외부에 있고, 무시된다. 위의 블록은 공백으로
    분리된 plain text 형식이고, 아래의 블록은 파이프로 분리된 영힉을
    사용한다.

    .. code:: robotframework

       | *** Keyword ***  |                        |         |
       | My Keyword       | [Arguments]            | ${path} |
       |                  | Directory Should Exist | ${path} |

..
   .. note:: Escaping_ using the backslash character works normally in this format.
	     No double escaping is needed like when using reST tables.

.. note:: 백슬래시 문자를 사용하는 Escaping_ 은 reST 형식에서 잘
          동작한다. reST 표에서는 escaping 문자를 두 번 쓸 필요가
          없다.

..
   .. note:: Support for test data in code blocks is a new feature in
	     Robot Framework 2.8.2.

.. note:: Robot Framework 2.8.2 부터는 코드 블록에서 테스트 데이터를
          지원한다.

Using tables
''''''''''''

..
   If a reStructuredText document contains no code blocks with Robot Framework
   data, it is expected to contain the data in tables similarly as in
   the `HTML format`_. In this case Robot Framework compiles the document to
   HTML in memory and parses it exactly like it would parse a normal HTML file.

만약 reStructuredText 문서가 Robot Framework 데이터가 있는 코드 블록이
없다면, `HTML format`_ 처럼 표에 데이터를 포함할 것이다. 이런 경우,
Robot Framework는 문서를 컴파일 하여 HTML로 메모리로 올리고 일반 HTML
파일과 동일하게 파싱한다.

..
   Robot Framework identifies `test data tables`_ based on the text in the first
   cell and all content outside of the recognized table types is ignored.
   An example of each of the four test data tables is shown below
   using both simple table and grid table syntax:

Robot Framework는 텍스트 기반의 `test data tables`_ 첫번째 셀에서
테이블 타입을 인지하고, 표 외부의 모든 내용은 무시한다. 4 가지 테스트
데이터 표에 대한 단순한 표와 격자무늬 표 synax를 이용한 각각의 예를
아래에 표현한다:

.. sourcecode:: rest

    Example
    -------

    이 텍스트는 표 밖에 위치해서 무시된다.

    ============  ================  =======  =======
      Setting          Value         Value    Value
    ============  ================  =======  =======
    Library       OperatingSystem
    ============  ================  =======  =======


    ============  ================  =======  =======
      Variable         Value         Value    Value
    ============  ================  =======  =======
    ${MESSAGE}    Hello, world!
    ============  ================  =======  =======


    =============  ==================  ============  =============
      Test Case          Action          Argument      Argument
    =============  ==================  ============  =============
    My Test        [Documentation]     Example test
    \              Log                 ${MESSAGE}
    \              My Keyword          /tmp
    \
    Another Test   Should Be Equal     ${MESSAGE}    Hello, world!
    =============  ==================  ============  =============

    이 텍스트는 표 외부에 위치해서 무시된다. 위의 표는 간단한 표
    syntax를 사용하여 생성했고 아래 표는 격자무늬 표를 사용하였다.

    +-------------+------------------------+------------+------------+
    |   Keyword   |         Action         |  Argument  |  Argument  |
    +-------------+------------------------+------------+------------+
    | My Keyword  | [Arguments]            | ${path}    |            |
    +-------------+------------------------+------------+------------+
    |             | Directory Should Exist | ${path}    |            |
    +-------------+------------------------+------------+------------+

..
   .. note:: Empty cells in the first column of simple tables need to be escaped.
	     The above example uses :codesc:`\\` but `..` could also be used.

.. note:: 단순한 표의 첫번째 셀이 빈 셀이라면 반드시 이스케이프되어야 한다.
          위의 예제는 :codesc:`\\` 와 `..` 를 사용할 수 있다.

..
   .. note:: Because the backslash character is an escape character in reST,
	     specifying a backslash so that Robot Framework will see it requires
	     escaping it with an other backslash like `\\`. For example,
	     a new line character must be written like `\\n`. Because
	     the backslash is used for escaping_ also in Robot Framework data,
	     specifying a literal backslash when using reST tables requires double
	     escaping like `c:\\\\temp`.

.. note:: 백슬래시 문자는 reST에서 이스케이프 문자로 사용되기 때문에,
          백슬래시를 명기하기 위해서 `\\` 와 같이 다른 백슬래시를 함께
          사용해야 한다. 예를들어 개행 문자는 `\\n` 와 같이 표현해야
          한다. 백슬래시는 Robot Framework 데이터에서 escaping_ 의
          용도로 사용되기 때문에, reST 표를 사용할 때 문자 그대로의
          백슬래쉬를 명기하기 위해서는 `c:\\\\temp` 와 같이 이중
          이스케이핑을 해야 한다.

..
   Generating HTML files based on reST files every time tests are run obviously
   adds some overhead. If this is a problem, it can be a good idea to convert
   reST files to HTML using external tools separately, and let Robot Framework
   use the generated files only.

매 테스트 마다 reST 파일 기반의 HTML 파일을 생성하는것은 부하를
일으킨다. 만약 이것이 문제라면, 외부 툴을 별도로 이용하여, reST 파일을
HTML로 변환하고, Robot Framework가 생성된 파일만 사용하게 하는 것이
좋다.

Editing and encoding
''''''''''''''''''''

..
   Test data in reStructuredText files can be edited with any text editor, and
   many editors also provide automatic syntax highlighting for it. reST format
   is not supported by RIDE_, though.

reStructuredText 파일내의 테스트 데이터는 텍스트 에디터를
편집가능하고, 많은 에디터가 자동 문법 하이라이팅 기능을 제공한다.
하지만 RIDE_ 는 reST 형식을 지원하지 않는다.

..
   Robot Framework requires reST files containing non-ASCII characters to be
   saved using UTF-8 encoding.

Robot Framework는 UTF-8 인코딩을 사용하기때문에 non-ASCII 문자를
포함하는 reST 파일은 반드시 UTF-8 인코딩으로 저장해야 한다.

Syntax errors in reST source files
''''''''''''''''''''''''''''''''''

..
   If a reStructuredText document is not syntactically correct (a malformed table
   for example), parsing it will fail and no test cases can be found from that
   file. When executing a single reST file, Robot Framework will show the error
   on the console. When executing a directory, such parsing errors will
   generally be ignored.

만일 reStructuredText 문서가 문법적으로 옳지 않다면 (예를 들어 표
형식이 잘못되었다면), 불러오기는 실패하고, 파일에서 테스트 케이스를
찾을 수 없다. 단일 reST 파일을 실행할 때, Robot Framework는 콘솔에
에러를 출력할 것이다. 디렉토리를 실행할 경우 일반적으로 파싱 에러는
무시된다.

..
   Starting from Robot Framework 2.9.2, errors below level `SEVERE` are ignored
   when running tests to avoid noise about non-standard directives and other such
   markup. This may hide also real errors, but they can be seen when processing
   files normally.

Robot Framework 2.9.2부터 표준이 아닌 지시자와 마크업에 대한 노이즈를
피하기 위해 `SEVERE` 이하의 에러는 무시한다. 이 경우 진짜 에러가
감춰질 수 있지만 보통 파일로 수행할 때는 볼 수 있다.

Test data tables
----------------

..
   Test data is structured in four types of tables listed below. These
   test data tables are identified by the first cell of the table. Recognized
   table names are `Settings`, `Variables`, `Test Cases`, and `Keywords`. Matching
   is case-insensitive and also singular variants like `Setting` and `Test Case`
   are accepted.

테스트 데이터는 아래와 같이 네가지 타입의 표로 구성된다. 이러한 테스트
데이터 표는 첫번째 셀에 의해 구분된다. 인식되는 표의 이름은
`Settings`, `Variables`, `Test Cases`, `Keywords` 이다. 대소문자
구분을 하지 않고, 단수 `Setting` 와 `Test Case` 도 인식한다.

.. table:: Different test data tables
   :class: tabular

   +--------------+--------------------------------------------+
   |    Table     |                 Used for                   |
   +==============+============================================+
   | Settings     | | 1) Importing `test libraries`_,          |
   |              |   `resource files`_ 와 `variable files`_.  |
   |              | | 2) `test suites`_ 와 `test cases`_ 를    |
   |              |   위한 metadata 정의                       |
   +--------------+--------------------------------------------+
   | Variables    | 테스트 데이타 안에서 사용할 variables_ 정의|
   +--------------+--------------------------------------------+
   | Test Cases   | 키워드를 사용하여 `Creating test cases`_   |
   +--------------+--------------------------------------------+
   | Keywords     | 이미 존재하는 lower-level 키워드로         |
   |              | `Creating user keywords`_                  |
   +--------------+--------------------------------------------+

Rules for parsing the data
--------------------------

.. _comment:

Ignored data
~~~~~~~~~~~~

..
   When Robot Framework parses the test data, it ignores:

Robot Framework가 테스트 데이터를 파싱할 때, 아래는 무시한다:

..
   - All tables that do not start with a `recognized table name`__ in the first cell.
   - Everything else on the first row of a table apart from the first cell.
   - All data before the first table. If the data format allows data between
     tables, also that is ignored.
   - All empty rows, which means these kinds of rows can be used to make
     the tables more readable.
   - All empty cells at the end of rows, unless they are escaped__.
   - All single backslashes (:codesc:`\\`) when not used for escaping_.
   - All characters following the hash character (`#`), when it is the first
     character of a cell. This means that hash marks can be used to enter
     comments in the test data.
   - All formatting in the HTML/reST test data.

- 첫번째 셀이 `인식가능한 표 이름`__ 으로 시작하지 않는 모든 표
- 인식되어진 표의 첫번째 줄에서 첫번째 셀을 제외한 모든 것
- 첫번째 표 이전의 모든 데이터. 데이터 포맷이 표와 표 사이의 데이터를
  허용한 경우에도 무시한다.
- 빈 줄 (테이블에서 사용된 빈줄은 가독성을 좋게 한다.)
- 줄의 맨 마지막에 있는 빈 셀. escaped__ 된 셀도 무시.
- escaping_ 목적이 아닌, 한 개의 백슬래시 (:codesc:`\\`)
- 셀안의 첫번째 문자가 해시 문자(`#`)일 경우 그 뒤에 오는 문자. 해시
  문자는 주석 표시 용으로 사용
- HTML/reST 테스트 데이터의 모든 서식

..
   When Robot Framework ignores some data, this data is not available in
   any resulting reports and, additionally, most tools used with Robot
   Framework also ignore them. To add information that is visible in
   Robot Framework outputs, place it to the documentation or other metadata of
   test cases or suites, or log it with the BuiltIn_ keywords :name:`Log` or
   :name:`Comment`.

Robot Framework가 몇몇 데이터를 무시할 경우, 결과 리포트에서도 이
데이터는 사용할 수 없다. 추가적으로 Robot Framework와 함께 사용되는
대부분의 툴도 같이 무시한다. Robot Framework 결과에 정보를 추가하기
위해서는 documentaion이나 test case 또는 suite의 metadata에
작성하거나, BuiltIn_ 키워드인 :name:`Log` 또는 :name:`Comment` 를
사용하여 로그를 작성할 수 있다.

__ `Test data tables`_
__ `Prevent ignoring empty cells`_

Handling whitespace
~~~~~~~~~~~~~~~~~~~

..
   Robot Framework handles whitespace the same way as they are handled in HTML
   source code:

Robot Framework는 HTML 소스코드와 같은 방법으로 공백문자(whitespace)
[#]_ 을 다룬다:

..
   - Newlines, carriage returns, and tabs are converted to spaces.
   - Leading and trailing whitespace in all cells is ignored.
   - Multiple consecutive spaces are collapsed into a single space.

- 새로운 줄, 캐리지 리턴, 탭은 공백으로 변환
- 셀 앞, 뒤의 공백문자 무시
- 여러개 연속적인 공백은 한 칸으로 축소

..
   In addition to that, non-breaking spaces are replaced with normal spaces.
   This is done to avoid hard-to-debug errors
   when a non-breaking space is accidentally used instead of a normal space.

추가적으로, 줄 바꿈 없는 공백 [#]_ 은 일반 공백으로 치환된다. 이렇게
함으로서, 줄 바꿈 없는 공백이 우연히 일반 공백 대신 사용되었을 때
발생하는 어려운 디버깅 에러를 회피한다.

..
   If leading, trailing, or consecutive spaces are needed, they `must be
   escaped`__. Newlines, carriage returns, tabs, and non-breaking spaces can be
   created using `escape sequences`_ `\n`, `\r`, `\t`, and `\xA0` respectively.


만약 앞, 뒤, 연속적인 공백들이 필요하다면 `반드시 이스케이프`__ 되어야
한다. 새로운 열, 캐리지 리턴, 탭, 줄바꿈 없는 공백은 각각 `escape
sequences`_ `\n`, `\r`, `\t`, `\xA0` 을 사용한다.

__ `Prevent ignoring spaces`_

.. [#] 통상적으로, 스페이스 바, 탭문자, 폼피드, 캐리지리턴(Carriage
       Return), 개행문자(Newline) 등을 총칭함

.. [#] `non-breaking space
       <https://ko.wikipedia.org/wiki/%EC%A4%84_%EB%B0%94%EA%BF%88_%EC%97%86%EB%8A%94_%EA%B3%B5%EB%B0%B1>`_

Escaping
~~~~~~~~

..
   The escape character in Robot Framework test data is the backslash
   (:codesc:`\\`) and additionally `built-in variables`_ `${EMPTY}` and `${SPACE}`
   can often be used for escaping. Different escaping mechanisms are
   discussed in the sections below.

Robot Framework 테스트 데이터에서 이스케이프 문자는
백슬래시(:codesc:`\\`)다. 추가적으로 `built-in variables`_ `${EMPTY}`
와 `${SPACE}` 도 이스케이프로 사용할 수 있다. 다른 이스케이프
메카니즘은 아래 섹션에서 논의할 것이다.

Escaping special characters
'''''''''''''''''''''''''''

..
   The backslash character can be used to escape special characters
   so that their literal values are used.

백슬래시 문자는 특별한 문자를 문자 그대로의 값으로 사용하기 위해
이스케이프 하는데에 사용한다.

.. table:: Escaping special characters
   :class: tabular

   ===========  ================================================================  ==============================
    Character                              Meaning                                           Examples
   ===========  ================================================================  ==============================
   `\$`         Dollar sign, never starts a `scalar variable`_.                   `\${notvar}`
   `\@`         At sign, never starts a `list variable`_.                         `\@{notvar}`
   `\%`         Percent sign, never starts an `environment variable`_.            `\%{notvar}`
   `\#`         Hash sign, never starts a comment_.                               `\# not comment`
   `\=`         Equal sign, never part of `named argument syntax`_.               `not\=named`
   `\|`         Pipe character, not a separator in the `pipe separated format`_.  `| Run | ps \| grep xxx |`
   `\\`         Backslash character, never escapes anything.                      `c:\\temp, \\${var}`
   ===========  ================================================================  ==============================

.. _escape sequence:
.. _escape sequences:

Forming escape sequences
''''''''''''''''''''''''

..
   The backslash character also allows creating special escape sequences that are
   recognized as characters that would otherwise be hard or impossible to create
   in the test data.

백슬래시 문자는 특별한 이스케이프 문을 생성해서 테스트 데이터에서
생성하기 어렵거나 불가능한 문자를 인식하게 한다.


.. table:: Escape sequences
   :class: tabular

   =============  ====================================  ============================
      Sequence                  Meaning                           Examples
   =============  ====================================  ============================
   `\n`           Newline character.                    `first line\n2nd line`
   `\r`           Carriage return character             `text\rmore text`
   `\t`           Tab character.                        `text\tmore text`
   `\xhh`         Character with hex value `hh`.        `null byte: \x00, ä: \xE4`
   `\uhhhh`       Character with hex value `hhhh`.      `snowman: \u2603`
   `\Uhhhhhhhh`   Character with hex value `hhhhhhhh`.  `love hotel: \U0001f3e9`
   =============  ====================================  ============================

..
   .. note:: All strings created in the test data, including characters like
	     `\x02`, are Unicode and must be explicitly converted to
	     byte strings if needed. This can be done, for example, using
	     :name:`Convert To Bytes` or :name:`Encode String To Bytes` keywords
	     in BuiltIn_ and String_ libraries, respectively, or with
	     something like `str(value)` or `value.encode('UTF-8')`
	     in Python code.

.. note:: `\x02` 같은 문자를 포함한 테스트 데이터의 모든 스트링은
          유니코드이며, 필요에 따라 명시적으로 바이트 스트링으로
          변환해야 한다. 예를 들어, BuiltIn_ 와 String_ 라이브러리의
          :name:`Convert To Bytes` 혹은 :name:`Encode String To Bytes`
          키워드를 이용하여, 파이썬 코드의 `str(value)` 혹은
          `value.encode('UTF-8')` 로 만들 수 있다.

..
   .. note:: If invalid hexadecimal values are used with `\x`, `\u`
	     or `\U` escapes, the end result is the original value without
	     the backslash character. For example, `\xAX` (not hex) and
	     `\U00110000` (too large value) result with `xAX`
	     and `U00110000`, respectively. This behavior may change in
	     the future, though.

.. note:: 만약 유효하지 않은 16진법의 값이 `\x`, `\u`, `\U` 를
          이용하여 표현되었다면, 최종 결과값은 백슬래시 문자를 제외한
          원래의 값이 될 것이다. 예를 들어, `\xAX` (not hex)는 `xAX`
          로, `\U00110000` (too large value) 는 `U00110000` 가 될
          것이다. 이런 동작은 물론 나중에는 변경될 수도 있다.

..
   .. note:: `Built-in variable`_ `${\n}` can be used if operating system
	     dependent line terminator is needed (`\r\n` on Windows and
	     `\n` elsewhere).

.. note:: `Built-in variable`_ `${\n}` 은 OS에 의존적인 라인 종결자로
          사용할 수 있다. (윈도우즈에서는 `\r\n` 그리고 나머지는
          `\n`).

..
   .. note:: Possible un-escaped whitespace character after the `\n` is
	     ignored. This means that `two lines\nhere` and
	     `two lines\n here` are equivalent. The motivation for this
	     is to allow wrapping long lines containing newlines when using
	     the HTML format, but the same logic is used also with other formats.
	     An exception to this rule is that the whitespace character is not
	     ignored inside the `extended variable syntax`_.

.. note:: `\n` 뒤의 이스케이프 되지 않은 공백문자는 무시된다. 이말은
          곧, `two lines\nhere` 와 `two lines\n here` 는 동일하다는
          것이다. 이것은 HTML 형식을 이용할 때, 개행을 포함한 긴 줄을
          가능하도록 한다. 이것은 다른 형식에서도 이용 가능하다. 이
          규칙의 예외는 공백문자가 `extended variable syntax`_ 에서는
          무시되지 않는다는 것이다.

..
   .. note:: `\x`, `\u` and `\U` escape sequences are new in Robot Framework 2.8.2.

.. note:: `\x`, `\u`, `\U` 는 Robot Framework 2.8.2부터 새로 도입된
          이스케이프 문이다.

Prevent ignoring empty cells
''''''''''''''''''''''''''''

..
   If empty values are needed as arguments for keywords or otherwise, they often
   need to be escaped to prevent them from being ignored__. Empty trailing cells
   must be escaped regardless of the test data format, and when using the
   `space separated format`_ all empty values must be escaped.

만약 키워드의 전달인자로 빈 값이 필요하다면, 무시__ 되는 것을 막기
위해 이스케이프할 필요가 있다. 테스트 데이터 형식에 상관 없이 뒤에 빈
셀이 필요하다면 반드시 이스케이프 되어야 한다. 그리고 `space separated
format`_ 을 사용할때 모든 빈 값은 이스케이프 되어야만 한다.

..
   Empty cells can be escaped either with the backslash character or with
   `built-in variable`_ `${EMPTY}`. The latter is typically recommended
   as it is easier to understand. An exception to this recommendation is escaping
   the indented cells in `for loops`_ with a backslash when using the
   `space separated format`_. All these cases are illustrated in the following
   examples first in HTML and then in the space separated plain text format:

빈 셀은 백슬래시 문자 혹은 `built-in variable`_ `${EMPTY}` 을 이용해서
이스케이프 한다. 후자가 더 이해하기 쉽기 때문에 많이 추천한다. 하지만
예외적으로 `space separated format`_ 사용시 `for loops`_ 내의 들여쓰는
셀은 백슬래시로 이스케이프한다. 이 모든 케이스에 대한 HTML 및 the
space separated plain text 형식의 예제는 아래와 같다:

.. table::
   :class: example

   ==================  ============  ==========  ==========  ================================
        Test Case         Action      Argument    Argument                Argument
   ==================  ============  ==========  ==========  ================================
   Using backslash     Do Something  first arg   \\
   Using ${EMPTY}      Do Something  first arg   ${EMPTY}
   Non-trailing empty  Do Something              second arg  # No escaping needed in HTML
   For loop            :FOR          ${var}      IN          @{VALUES}
   \                                 Log         ${var}      # No escaping needed here either
   ==================  ============  ==========  ==========  ================================

.. sourcecode:: robotframework

   *** Test Cases ***
   Using backslash
       Do Something    first arg    \
   Using ${EMPTY}
       Do Something    first arg    ${EMPTY}
   Non-trailing empty
       Do Something    ${EMPTY}     second arg    # Escaping needed in space separated format
   For loop
       :FOR    ${var}    IN    @{VALUES}
       \    Log    ${var}                         # Escaping needed here too

__ `Ignored data`_

Prevent ignoring spaces
'''''''''''''''''''''''

..
   Because leading, trailing, and consecutive spaces in cells are ignored__, they
   need to be escaped if they are needed as arguments to keywords or otherwise.
   Similarly as when preventing ignoring empty cells, it is possible to do that
   either using the backslash character or using `built-in variable`_
   `${SPACE}`.

셀에서 앞, 뒤, 연속된 공백은 무시__ 되기 때문에, 만일 공백을
전달인자로서 키워드나 다른곳에 사용하고자 한다면 반드시 이스케이프
되어야 한다. 빈 셀이 무시되는것을 방지하기 위해서, 백슬래시 문자를
사용하거나 `built-in variable`_ `${SPACE}` 를 사용해야 한다.

.. table:: Escaping spaces examples
   :class: tabular

   ==================================  ==================================  ==================================
        Escaping with backslash             Escaping with `${SPACE}`                      Notes
   ==================================  ==================================  ==================================
   :codesc:`\\ leading space`          `${SPACE}leading space`
   :codesc:`trailing space \\`         `trailing space${SPACE}`            Backslash must be after the space.
   :codesc:`\\ \\`                     `${SPACE}`                          Backslash needed on both sides.
   :codesc:`consecutive \\ \\ spaces`  `consecutive${SPACE * 3}spaces`     Using `extended variable syntax`_.
   ==================================  ==================================  ==================================

..
   As the above examples show, using the `${SPACE}` variable often makes the
   test data easier to understand. It is especially handy in combination with
   the `extended variable syntax`_ when more than one space is needed.

위의 예제에서 보였듯이, `${SPACE}` 변수를 사용하면 테스트 데이터를
이해하기 쉽다. 특히 한칸 이상의 공백이 필요한 경우, `extended
variable syntax`_ 와 함께 사용하면 유용하다.

__ `Handling whitespace`_


Dividing test data to several rows
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   If there is more data than readily fits a row, it possible to use ellipsis
   (`...`) to continue the previous line. In test case and keyword tables,
   the ellipsis must be preceded by at least one empty cell.  In settings and
   variable tables, it can be placed directly under the setting or variable name.
   In all tables, all empty cells before the ellipsis are ignored.

만약 한줄에 적합한 길이보다 더 많은 데이터가 있다면, 이전 줄과
연결하기 위해서 생략부호(`...`)을 사용할 수 있다. Test case와 keyword
표에서, 생략부호는 첫번째 셀에 적어야 한다. Setting과 Variable 표에서,
생략부호는 setting과 variable 이름 아래에 존재할 수 있다. 모든 표에서,
생략부호 앞의 모든 빈 셀은 무시한다.

..
   Additionally, values of settings that take only one value (mainly
   documentations) can be split to several columns. These values will be
   then catenated together with spaces when the test data is
   parsed. Starting from Robot Framework 2.7, documentation and test
   suite metadata split into multiple rows will be `catenated together
   with newlines`__.

추가적으로, 오직 한 값만 가지는 setting 값들은(주로 documentations)
여러 열로 분리될 수 있다. 이 값들은 테스트 데이터가 파싱될 때 공백과
함께 이어 붙인다. Robot Framework 2.7부터, 여러 줄로 분리되는
documentation과 test suite metadata는 `catenated together with
newlines`__ 된다.

..
   All the syntax discussed above is illustrated in the following examples.
   In the first three tables test data has not been split, and
   the following three illustrate how fewer columns are needed after
   splitting the data to several rows.

위에서 다룬 모든 문법은 아래에서 예제로 보인다. 테스트 데이터의 처음
세개의 표는 분리하지 않은 것이고, 다음의 세개의 표는 데이터를 여러줄로
분할 한 뒤, 얼마나 적은 열만 필요한지 보여준다.

__ `Newlines in test data`_

.. table:: Test data that has not been split
   :class: example

   ============  =======  =======  =======  =======  =======  =======
     Setting      Value    Value    Value    Value    Value    Value
   ============  =======  =======  =======  =======  =======  =======
   Default Tags  tag-1    tag-2    tag-3    tag-4    tag-5    tag-6
   ============  =======  =======  =======  =======  =======  =======

.. table::
   :class: example

   ==========  =======  =======  =======  =======  =======  =======
    Variable    Value    Value    Value    Value    Value    Value
   ==========  =======  =======  =======  =======  =======  =======
   @{LIST}     this     list     has      quite    many     items
   ==========  =======  =======  =======  =======  =======  =======

.. table::
   :class: example

   +-----------+-----------------+---------------+------+-------+------+------+-----+-----+
   | Test Case |     Action      |   Argument    | Arg  |  Arg  | Arg  | Arg  | Arg | Arg |
   +===========+=================+===============+======+=======+======+======+=====+=====+
   | Example   | [Documentation] | Documentation |      |       |      |      |     |     |
   |           |                 | for this test |      |       |      |      |     |     |
   |           |                 | case.\\n This |      |       |      |      |     |     |
   |           |                 | can get quite |      |       |      |      |     |     |
   |           |                 | long...       |      |       |      |      |     |     |
   +-----------+-----------------+---------------+------+-------+------+------+-----+-----+
   |           | [Tags]          | t-1           | t-2  | t-3   | t-4  | t-5  |     |     |
   +-----------+-----------------+---------------+------+-------+------+------+-----+-----+
   |           | Do X            | one           | two  | three | four | five | six |     |
   +-----------+-----------------+---------------+------+-------+------+------+-----+-----+
   |           | ${var} =        | Get X         | 1    | 2     | 3    | 4    | 5   | 6   |
   +-----------+-----------------+---------------+------+-------+------+------+-----+-----+

.. table:: Test data split to several rows
   :class: example

   ============  =======  =======  =======
     Setting      Value    Value    Value
   ============  =======  =======  =======
   Default Tags  tag-1    tag-2    tag-3
   ...           tag-4    tag-5    tag-6
   ============  =======  =======  =======

.. table::
   :class: example

   ==========  =======  =======  =======
    Variable    Value    Value    Value
   ==========  =======  =======  =======
   @{LIST}     this     list     has
   ...         quite    many     items
   ==========  =======  =======  =======

.. table::
   :class: example

   ===========  ================  ==============  ==========  ==========
    Test Case       Action           Argument      Argument    Argument
   ===========  ================  ==============  ==========  ==========
   Example      [Documentation]   Documentation   for this    test case.
   \            ...               This can get    quite       long...
   \            [Tags]            t-1             t-2         t-3
   \            ...               t-4             t-5
   \            Do X              one             two         three
   \            ...               four            five        six
   \            ${var} =          Get X           1           2
   \                              ...             3           4
   \                              ...             5           6
   ===========  ================  ==============  ==========  ==========
