Creating user keywords
======================

..
   Keyword tables are used to create new higher-level keywords by
   combining existing keywords together. These keywords are called *user
   keywords* to differentiate them from lowest level *library keywords*
   that are implemented in test libraries. The syntax for creating user
   keywords is very close to the syntax for creating test cases, which
   makes it easy to learn.

Keyword 표는 이미 존재하는 키워드를 조합하여 새로운 higher-level
키워드를 만드는 데 사용한다. 이런 키워드는 테스트 라이브러리에 구현된
가장 낮은 수준의 *library keywords* 와 구분하기 위해 *user keywords*
로 부른다. user keywords를 만드는 syntax는 test cases를 생성하는
syntax와 유사하여 배우기 쉽다.

.. contents::
   :depth: 2
   :local:

User keyword syntax
-------------------

Basic syntax
~~~~~~~~~~~~

..
   In many ways, the overall user keyword syntax is identical to the
   `test case syntax`_.  User keywords are created in keyword tables
   which differ from test case tables only by the name that is used to
   identify them. User keyword names are in the first column similarly as
   test cases names. Also user keywords are created from keywords, either
   from keywords in test libraries or other user keywords. Keyword names
   are normally in the second column, but when setting variables from
   keyword return values, they are in the subsequent columns.

여러가지 면에서 user keyword syntax는 `test case syntax`_ 와 동일하다.
user keywords는 keyword 표에 생성하고, test case 표에서 단지
이름만으로 구분한다. 또한 user keywords는 테스트 라이브러리의
keywords나 다른 user keywords의 조합으로 생성한다. Keyword 이름은 보통
두번째 열에 위치한다. 하지만, keyword return values로 변수를 설정할 때
keyword 이름은 그 다음 칸에 위치한다.

.. sourcecode:: robotframework

   *** Keywords ***
   Open Login Page
       Open Browser    http://host/login.html
       Title Should Be    Login Page

   Title Should Start With
       [Arguments]    ${expected}
       ${title} =    Get Title
       Should Start With    ${title}    ${expected}

..
   Most user keywords take some arguments. This important feature is used
   already in the second example above, and it is explained in detail
   `later in this section`__, similarly as `user keyword return
   values`_.

대부분의 user keywords는 전달인자를 가진다. 이러한 주요 기능은 이미
위의 두번째 예제에서 사용했다. `본 섹션내에서의 뒤에서`__ `user
keyword return values`_ 와 같이 상세히 설명한다.

__ `User keyword arguments`_

..
   User keywords can be created in `test case files`_, `resource files`_,
   and `test suite initialization files`_. Keywords created in resource
   files are available for files using them, whereas other keywords are
   only available in the files where they are created.

User keywords는 `test case files`_, `resource files`_, `test suite
initialization files`_ 에 생성한다. Resource files에 생성된 키워드는
이것을 사용(import)하는 파일에서 사용할 수 있다. 반면에 다른 키워드는
생성된 파일안에서만 사용할 수 있다.
   
Settings in the Keyword table
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   User keywords can have similar settings as `test cases`__, and they
   have the same square bracket syntax separating them from keyword
   names. All available settings are listed below and explained later in
   this section.

User keywords는 `test cases`__ 와 비슷하게 설정하고, 키워드 이름과
구분하기 위해서 대괄호([]) syntax를 사용한다. 모든 가능한 설정을 아래에
보이고 본 섹션의 뒤에서 설명한다.

`[Documentation]`:setting:
  `User keyword documentation`_ 를 성성하기 위해 사용

..
     Used for setting a `user keyword documentation`_.
   
   
`[Tags]`:setting:
  키워드의 `tags`__ 설정

..
     Sets `tags`__ for the keyword.

`[Arguments]`:setting:
   `User keyword arguments`_ 기술

..   
   Specifies `user keyword arguments`_.

`[Return]`:setting:
   `User keyword return values`_ 기술

..
   Specifies `user keyword return values`_.

`[Teardown]`:setting:
   `User keyword teardown`_ 기술

..
   Specify `user keyword teardown`_.

`[Timeout]`:setting:
   `User keyword timeout`_ 설정. Timeouts_ 별도 섹션에서 설명

..
   Sets the possible `user keyword timeout`_. Timeouts_ are discussed
   in a section of their own.

__ `Settings in the test case table`_
__ `User keyword tags`_

.. _User keyword documentation:

User keyword name and documentation
-----------------------------------

..
   The user keyword name is defined in the first column of the user
   keyword table. Of course, the name should be descriptive, and it is
   acceptable to have quite long keyword names. Actually, when creating
   use-case-like test cases, the highest-level keywords are often
   formulated as sentences or even paragraphs.

User keyword 이름은 user keyword 표의 첫번째 열에 정의한다. 물론 그
이름은 키워드를 잘 설명해야 하기 때문에 아주 긴 이름도 허용한다.
실제로 use-case-like 테스트 케이스를 생성할 때 highest-level
keywords는 문장이나 심지어 단락으로 기술할 수도 있다.

..
   User keywords can have a documentation that is set with the
   :setting:`[Documentation]` setting, exactly as `test case documentation`_.
   This setting documents the user keyword in the test data. It is also shown
   in a more formal keyword documentation, which the Libdoc_ tool can create
   from `resource files`_. Finally, the first row of the documentation is
   shown as a keyword documentation in `test logs`_.

User keywords는 `test case documentation`_ 과 동일하게
:setting:`[Documentation]` 설정에 문서화할 수 있다. 이런 설정은
테스트 테이터내에서 user keyword를 문서화한다. 더욱 자세한 것은
Libdoc_ 툴이 `resource files`_ 로부터 생성한 공식적인 keyword
documetation에서 볼 수 있다. 마지막으로 documentaion의 첫 줄은 `test
logs`_ 에서 keyword documentation으로 출력된다.

..
   Sometimes keywords need to be removed, replaced with new ones, or
   deprecated for other reasons.  User keywords can be marked deprecated
   by starting the documentation with `*DEPRECATED*`, which will
   cause a warning when the keyword is used. For more information, see
   `Deprecating keywords`_ section.

때때로 키워드는 삭제되거나 새로운 것으로 교체되거나 다른 여러가지
이유로 deprecated [#]_ 해야 한다. User keywords는 documentation에
`*DEPRECATED*` 로 시작하여 deprecated로 마크할수 있다. 이것은 해당
키워드를 사용할 때 경고를 발생한다. 더 자세한 정보를 `deprecating
keywords`_ 섹션을 참고하라.

.. [#] 중요도가 떨어져 더이상 사용되지 않고 사라질

User keyword tags
-----------------

..
   Starting from Robot Framework 2.9, keywords can also have tags. User keywords
   tags can be set with :setting:`[Tags]` setting similarly as `test case tags`_,
   but possible :setting:`Force Tags` and :setting:`Default Tags` setting do not
   affect them. Additionally keyword tags can be specified on the last line of
   the documentation with `Tags:` prefix and separated by a comma. For example,
   `Tags: tag1, tag2`.

Robot Framework 2.9에서 부터 키워드는 태크를 가질 수 있다. User
Keywords 태크는 `test case tags`_ 와 비슷하게 :setting:`[Tags]` 설정에
설정한다. :setting:`Force Tags` 와 :setting:`Default Tags` 설정은
영향을 미치지 않는다. 추가적으로 keyword 태그는 documentation의 마지막
라인에 `Tags:` 에 쉼표(,)로 구분하여 기술할 수도 있다. (예, `Tags:
tag1, tag2`)

..
   Keyword tags are shown in logs and in documentation generated by Libdoc_,
   where the keywords can also be searched based on tags. The `--removekeywords`__
   and `--flattenkeywords`__ commandline options also support selecting keywords by
   tag, and new usages for keywords tags are possibly added in later releases.

Keyword 태그는 로그와 Libdoc_ 으로 생성한 문서에 보여진다. 문서에서는
태그로 검색도 가능하다. `--removekeywords`__ 와 `--flattenkeywords`__
명령행 옵션은 태그로 키워드를 선택할 수 있다. keyword 태그에 대한
새로운 사용법은 이후의 배포본에 추가될 수 있다.

..
   Similarly as with `test case tags`_, user keyword tags with a `robot-` prefix
   are reserved__ for special features by Robot Framework itself. Users should
   thus not use any tag with a `robot-` prefix unless actually activating
   the special functionality.

`test case tags`_ 와 비슷하게 `robot-` 접두어를 가지는 user keyword
태그는 Robot Framework를 특별한 기능을 위해 예약되어__ 있다. 따라서
실제로 특별한 기능을 활성화하지 않는한 `robot--` 접두어를 가지는
태그는 사용하지 말아야 한다.

__ `Removing keywords`_
__ `Flattening keywords`_
__ `Reserved tags`_

User keyword arguments
----------------------

..
   Most user keywords need to take some arguments. The syntax for
   specifying them is probably the most complicated feature normally
   needed with Robot Framework, but even that is relatively easy,
   particularly in most common cases. Arguments are normally specified with
   the :setting:`[Arguments]` setting, and argument names use the same
   syntax as variables_, for example `${arg}`.

대부분의 user keywords는 전달인자를 가진다. 전달인자를 기술하는
syntax는 Robot Framework에서 가장 복잡하다. 하지만 보통의 경우
상대적으로 쉽다. 전달인자는 :setting:`[Arguments]` 설정에 기술하고,
이름은 variables_ syntax와 동일하게 사용한다. (예, `${arg}`)

Positional arguments
~~~~~~~~~~~~~~~~~~~~

..
   The simplest way to specify arguments (apart from not having them at all)
   is using only positional arguments. In most cases, this is all
   that is needed.

전달인자를 기술하는 가장 간단한 방법은 positional arguments를 사용하는
것이다. 대부분의 경우 이렇게 사용한다.

..
   The syntax is such that first the :setting:`[Arguments]` setting is
   given and then argument names are defined in the subsequent
   cells. Each argument is in its own cell, using the same syntax as with
   variables. The keyword must be used with as many arguments as there
   are argument names in its signature. The actual argument names do not
   matter to the framework, but from users' perspective they should
   be as descriptive as possible. It is recommended
   to use lower-case letters in variable names, either as
   `${my_arg}`, `${my arg}` or `${myArg}`.

:setting:`[Arguments]` 설정의 각셀에 전달인자 이름을 정의한다. 각각의
전달인자는 변수의 syntax와 같이 자신의 셀을 가진다. 키워드는
시그너처(signature) [#]_ 에 사용된 것과 동일한 수의 전달인자를
사용해야 한다. 프레임워크에서는 실제 전달인자 이름이 무엇이든
상관없지만, 사용자의 관점에서 가능하다면 설명적이어야 한다. 전달인자
이름은 `${my_arg}`, `${my arg}`, `${myArg}` 과 같이 소문자를 사용하는
것을 권장한다.

.. [#] `Type signature <https://en.wikipedia.org/wiki/Type_signature>`_ 를 참고하라.
       
.. sourcecode:: robotframework

   *** Keywords ***
   One Argument
       [Arguments]    ${arg_name}
       Log    Got argument ${arg_name}

   Three Arguments
       [Arguments]    ${arg1}    ${arg2}    ${arg3}
       Log    1st argument: ${arg1}
       Log    2nd argument: ${arg2}
       Log    3rd argument: ${arg3}

Default values with user keywords
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   When creating user keywords, positional arguments are sufficient in
   most situations. It is, however, sometimes useful that keywords have
   `default values`_ for some or all of their arguments. Also user keywords
   support default values, and the needed new syntax does not add very much
   to the already discussed basic syntax.

User keywords를 생성할 때 대부분의 경우 positional arguments로
충분하다. 하지만 경우에 따라 전달인자의 일부 또는 전부에 `default
values`_ 를 할당하면 유용하다. User keywords는 디폴트 값을 지원한다.
필요한 새로운 syntax는 이미 논의한 기본 syntax에 조금 만 추구하면 된다.

..
   In short, default values are added to arguments, so that first there is
   the equals sign (`=`) and then the value, for example `${arg}=default`.
   There can be many arguments with defaults, but they all must be given after
   the normal positional arguments. The default value can contain a variable_
   created on `suite or global scope`__.

디폴트 값을 전달인자에 할당하는 방법은 먼저 등호가 나오고 값을 적는다.
(예, `${arg}=default`) 여러개의 전달인자가 디폴트 값을 가질 수 있지만
반드시 position arguments뒤에 와야 한다. 디폴트 값은 `suite or global
scope`__ 로 생성된 variables_ 를 사용할 수 있다.

..
   .. note:: The syntax for default values is space sensitive. Spaces
	     before the `=` sign are not allowed, and possible spaces
	     after it are considered part of the default value itself.

.. note:: 디폴트 값에 대한 syntax는 공백에 민감하다. 반드시 `=` 부호
          앞에는 공백이 없어야 하며 뒤에는 공백은 디폴트 값의 일부로
          가능하다.

.. sourcecode:: robotframework

   *** Keywords ***
   One Argument With Default Value
       [Arguments]    ${arg}=default value
       [Documentation]    This keyword takes 0-1 arguments
       Log    Got argument ${arg}

   Two Arguments With Defaults
       [Arguments]    ${arg1}=default 1    ${arg2}=${VARIABLE}
       [Documentation]    This keyword takes 0-2 arguments
       Log    1st argument ${arg1}
       Log    2nd argument ${arg2}

   One Required And One With Default
       [Arguments]    ${required}    ${optional}=default
       [Documentation]    This keyword takes 1-2 arguments
       Log    Required: ${required}
       Log    Optional: ${optional}

..
   When a keyword accepts several arguments with default values and only
   some of them needs to be overridden, it is often handy to use the
   `named arguments`_ syntax. When this syntax is used with user
   keywords, the arguments are specified without the `${}`
   decoration. For example, the second keyword above could be used like
   below and `${arg1}` would still get its default value.

키워드가 디폴트 값을 가지는 여러개의 전달인가를 가지고 그 중 일부를
재정의(override) 할 때 가장 쉬운 방법은 `named arguments`_ syntax 를
사용하는 것이다. 이 syntax가 user keywords와 같이 사용할 때 전달인자는
반드시 `${}` 장식자 없이 적어야 한다. 예를 들어 위의 두번째 키워드는
아래 처럼 사용할 수 있다. 물론 `${arg1}` 은 여전히 디폴트 값을 사용한다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       Two Arguments With Defaults    arg2=new value

..
   As all Pythonistas must have already noticed, the syntax for
   specifying default arguments is heavily inspired by Python syntax for
   function default values.

모든 파이썬 사용자가 이미 알고 있듯이, 디폴트 전달인자를 기술하는
문법구조는 파이썬의 함수 기본 값(function default values) 문법구조에서
영감을 받았다.


__ `Variable priorities and scopes`_

Varargs with user keywords
~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Sometimes even default values are not enough and there is a need
   for a keyword accepting `variable number of arguments`_. User keywords
   support also this feature. All that is needed is having `list variable`_ such
   as `@{varargs}` after possible positional arguments in the keyword signature.
   This syntax can be combined with the previously described default values, and
   at the end the list variable gets all the leftover arguments that do not match
   other arguments. The list variable can thus have any number of items, even zero.

경우에 따라 디폴트 값만으로 충분하지 않아서 가변인자(`variable number
of arguments`_)를 가지는 키워드가 필요하다. User keywords는 이 기능을
지원한다. 이때 필요한 것은 키워드 시그너처에서 `list variable`_ 즉
`@{varargs}` 를 positional arguments뒤에 두면 된다. 이런 문법구조는
디폴트 값과 조합하여 끝에서 리스트 변수는 일치하지 않는 나머지 모든
인자를 얻을 수 있다. 그래서 리스트 변수는 0개를 포함하여 임의 갯수의
항목(item)을 가질 수 있다.

.. sourcecode:: robotframework

   *** Keywords ***
   Any Number Of Arguments
       [Arguments]    @{varargs}
       Log Many    @{varargs}

   One Or More Arguments
       [Arguments]    ${required}    @{rest}
       Log Many    ${required}    @{rest}

   Required, Default, Varargs
       [Arguments]    ${req}    ${opt}=42    @{others}
       Log    Required: ${req}
       Log    Optional: ${opt}
       Log    Others:
       : FOR    ${item}    IN    @{others}
       \    Log    ${item}

..
   Notice that if the last keyword above is used with more than one
   argument, the second argument `${opt}` always gets the given
   value instead of the default value. This happens even if the given
   value is empty. The last example also illustrates how a variable
   number of arguments accepted by a user keyword can be used in a `for
   loop`__. This combination of two rather advanced functions can
   sometimes be very useful.

위의 마지막 키워드는 하나이상을 키워드를 가진다. 두번째 인자
`${opt}`는 디폴트 값 대신 주어진 값을 얻을 수 있다. 만약 주어진 값이
빈값(empty)이어도 마찬가지다. 마지막 예제는 user keyword의 가변인자가
`for loop`__ 에서 사용되는 방법을 보여준다. 때때로 고급 기능보다
이러한 두가지 조합을 사용하는 것이 더 유용하다.
   
..
   Again, Pythonistas probably notice that the variable number of
   arguments syntax is very close to the one in Python.

가변인자문법 또한 파이썬의 문법구조와 비슷하다. 


__ `for loops`_

Kwargs with user keywords
~~~~~~~~~~~~~~~~~~~~~~~~~

..
   User keywords can also accept `free keyword arguments`_ by having a `dictionary
   variable`_ like `&{kwargs}` as the last argument after possible positional
   arguments and varargs. When the keyword is called, this variable will get all
   `named arguments`_ that do not match any positional argument in the keyword
   signature.

User keywords는 `free keyword arguments`_ 지원한다. 이것은 positional
arguments와 가변인자(varargs) 뒤에 `&{kwargs}` 같은 `dictionary
variable`_ 를 가진다. 키워드가 호출되었을 때 이 변수는 키워드
시그너처에서 임의의 positional argumetns와 일치하지 않는 모든 `named
arguments`_ 를 가진다.

.. sourcecode:: robotframework

   *** Keywords ***
   Kwargs Only
       [Arguments]    &{kwargs}
       Log    ${kwargs}
       Log Many    @{kwargs}

   Positional And Kwargs
       [Arguments]    ${required}    &{extra}
       Log Many    ${required}    @{extra}

   Run Program
       [Arguments]    @{varargs}    &{kwargs}
       Run Process    program.py    @{varargs}    &{kwargs}

..
   The last example above shows how to create a wrapper keyword that
   accepts any positional or named argument and passes them forward.
   See `kwargs examples`_ for a full example with same keyword.

위의 마지막 예제는 임의 갯수의 positional or named argument를 전달하는
wrapper 키워드를 만드는 방법을 보여준다. 자세한 내용은 `kwargs
examples`_ 을 참고하라.

..
   Also kwargs support with user keywords works very similarly as kwargs work
   in Python. In the signature and also when passing arguments forward,
   `&{kwargs}` is pretty much the same as Python's `**kwargs`.

kwargs가 파이썬에서 잘 동작하는 것 처럼 user keywords에서도 잘
동작한다. 시그너처 안에서와 인자를 전달 할때 `&{kwargs}` 는 파이썬의
`**kwargs` 와 거의 동일하다.

.. _Embedded argument syntax:

Embedding arguments into keyword name
-------------------------------------

..
   Robot Framework has also another approach to pass arguments to user
   keywords than specifying them in cells after the keyword name as
   explained in the previous section. This method is based on embedding
   the arguments directly into the keyword name, and its main benefit is
   making it easier to use real and clear sentences as keywords.

Robot Framework은 앞 절에서 설명한 것과(키워드 이름 다음 셀에 인자를
기술하여 user keywords에 전달하는 방법)는 다른 인자 전달방법이 있다.
이 방법은 키워드 이름 안에 직접적으로 인자들을 임베딩(embedding)한다.
이러한 방법의 주요 이점은 실제 명확한 문장을 가지는 키워드를 만들기가
더 쉽다는 것이다.

Basic syntax
~~~~~~~~~~~~

..
   It has always been possible to use keywords like :name:`Select dog
   from list` and :name:`Selects cat from list`, but all such keywords
   must have been implemented separately. The idea of embedding arguments
   into the keyword name is that all you need is a keyword with name like
   :name:`Select ${animal} from list`.

항상 :name:`Selecti dog from list` 와 :name:`Selects cat from list`
같은 키워드를 사용하는 것은 가능하지만, 이런 모든 키워드는 반드시 각각
구현되어야 한다. 키워드 이름 내에 인자를 임베딩하는 아이디어에서 필요로
하는 것은 :name:`Select ${animal} from list` 와 같은 키워드를 만드는
것이다.

.. sourcecode:: robotframework

   *** Keywords ***
   Select ${animal} from list
       Open Page    Pet Selection
       Select Item From List    animal_list    ${animal}

..
   Keywords using embedded arguments cannot take any "normal" arguments
   (specified with :setting:`[Arguments]` setting) but otherwise they are
   created just like other user keywords. The arguments used in the name
   will naturally be available inside the keyword and they have different
   value depending on how the keyword is called. For example,
   `${animal}` in the previous has value `dog` if the keyword
   is used like :name:`Select dog from list`. Obviously it is not
   mandatory to use all these arguments inside the keyword, and they can
   thus be used as wildcards.

임베드된 인자를 사용하는 키워드는 "일반적인"
인자(:setting:`[Arguments]` 설정에 기술하는)를 취할 수 없다. 하지만
그렇게 하지 않으면 단지 다른 user keywords 만드는 것과 같다. 이름안에
사용된 인자는 자연스럽게 키워드 내에서 사용가능하며 키워드가 호출되는
방법에 따라 서로 다른 값을 가진다. 예를 들면 앞의 예에서 `${animal}`
은 키워드가 :name:`Select dog from list` 처럼 사용되면 `dog` 값을
가진다. 분명히 키워드 내의 모든 인자를 사용하는 것이 필수 사항은
아니므로 이러한 변수는 와이드카드로 사용할 수 있다.

..
   These kind of keywords are also used the same way as other keywords
   except that spaces and underscores are not ignored in their
   names. They are, however, case-insensitive like other keywords. For
   example, the keyword in the example above could be used like
   :name:`select x from list`, but not like :name:`Select x fromlist`.

이런 종류의 키워드는 다른 키워드와 동일하게 사용가능하다. 대소문자는
구분하지 않는다. 하지만 공백과 언더스코어는 이름안에서 무시되지
않는다. 예를 들어 위의 예제에서 :name:`select x from list` 는 같이
사용할 수 있지만 :name:`Select x fromlist` 는 같이 사용할 수 없다.

..
   Embedded arguments do not support default values or variable number of
   arguments like normal arguments do. Using variables when
   calling these keywords is possible but that can reduce readability.
   Notice also that embedded arguments only work with user keywords.

임베디드 인자는 디폴트 값으니 가변인자를 지원하지 않는다. 이 키워드를
호출할 때 변수를 사용하는 것은 가능하지만 가독성을 줄일 수 있다.
임베디드 인자는 단지 사용자 키워드와 동작한다는 것에 주의하라.

Embedded arguments matching too much
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   One tricky part in using embedded arguments is making sure that the
   values used when calling the keyword match the correct arguments. This
   is a problem especially if there are multiple arguments and characters
   separating them may also appear in the given values. For example,
   keyword :name:`Select ${city} ${team}` does not work correctly if used
   with city containing too parts like :name:`Select Los Angeles Lakers`.


임베디드 인자를 사용할 때 까다로운 부분은 키워드를 호출할때 사용하는
값이 인수와 정확히 일치해야 한다는 것이다. 이것은 특히 여러개의 인수와
그것을 구분하는 문자가 주어진 값에 나타날 경우 문제가 된다. 예를 들면,
:name:`Select ${city} ${team}` 키워드는 만약 city가 :name:`Select Log
Angeles Lakers` 와 같이 두 부분으로 나눈어 진다면 제대로 동작하지
않는다.

..
   An easy solution to this problem is quoting the arguments (e.g.
   :name:`Select "${city}" "${team}"`) and using the keyword in quoted
   format (e.g. :name:`Select "Los Angeles" "Lakers"`). This approach is
   not enough to resolve all this kind of conflicts, though, but it is
   still highly recommended because it makes arguments stand out from
   rest of the keyword. A more powerful but also more complicated
   solution, `using custom regular expressions`_ when defining variables,
   is explained in the next section. Finally, if things get complicated,
   it might be a better idea to use normal positional arguments instead.

이 문제에 대한 쉬운 해결책은 인수에 인용부호(예, :name:`Select
"${city}" "$team}"`)를 사용하고 인용 형식으로 키워드를 사용하는
것이다(예, :name:`Selct "Los Angeles" "Lakers"`). 그러나 이 방법만으로
모든 종류의 충돌을 해결할 수 없다. 하지만 키워드의 나머지 부분에서
인자를 눈에 띄게 만들기 때문에 여전히 권장할만 하다. 더욱 강력하지만
복잡한 해결책은 다음 절에서 소개할 변수 정의시 `맞춤형(custom) 정규
표현식을 사용`__ 하는 것이다. 마지막으로 이것이 더 복잡해지면 보통의
positional arguements를 사용하는 것이 더 나을 수 있다.

__ `using custom regular expressions`_

..
   The problem of arguments matching too much occurs often when creating
   keywords that `ignore given/when/then/and/but prefixes`__ . For example,
   :name:`${name} goes home` matches :name:`Given Janne goes home` so
   that `${name}` gets value `Given Janne`. Quotes around the
   argument, like in :name:`"${name}" goes home`, resolve this problem
   easily.

`given/when/then/and/but 접두어를 무시하는`__ 키워드를 생설할 때, 너무
많이 일치하는 전달인자 문제가 발생한다. 예를 들면, :name:`${name} goes
home` 키워드는 :name:`Given Janne goes home` 키워드와 일치한다. 그래서
`${name}` 은 `Given Janne` 값을 가진다. :name:`"${name}" goes home` 와
같이 인자의 인용부호(")는 이 문제를 쉽게 해결한다. [#]_

__ `Ignoring Given/When/Then/And/But prefixes`_

.. [#] Given "Janne" goes home

Using custom regular expressions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
..
   When keywords with embedded arguments are called, the values are
   matched internally using `regular expressions`__
   (regexps for short). The default logic goes so that every argument in
   the name is replaced with a pattern `.*?` that basically matches
   any string. This logic works fairly well normally, but as just
   discussed above, sometimes keywords `match more than
   intended`__. Quoting or otherwise separating arguments from the other
   text can help but, for example, the test below fails because keyword
   :name:`I execute "ls" with "-lh"` matches both of the defined
   keywords.

임베디드 인자를 가지는 키워드가 호출되는 경우, 값은 내부적으로 `정규
표현식`__ (줄여서 regexps)을 사용하여 일치시킨다. 기본로직은 이름내의
모든 인자가 기본적으로 임의의 문자열과 일치하는 `.*?` 패턴을 가지고
대체 할 수 있도록 진행한다. 이 로직은 위에서 논의한 바와 `의도한
것보다 더 많이 일치하는`__ 몇몇 키워드에서도 꽤 잘 동작한다.
인용부호나 다른 텍스트로부터 인수를 분리하는 것은 도움이 되지만 아래
테스트는 실패한다. 왜냐하면 :name:`I execute "ls" with "-lh"` 키워드는
정의된 두 키워드와 모두 일치한다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       I execute "ls"
       I execute "ls" with "-lh"

   *** Keywords ***
   I execute "${cmd}"
       Run Process    ${cmd}    shell=True

   I execute "${cmd}" with "${opts}"
       Run Process    ${cmd} ${opts}    shell=True

..
   A solution to this problem is using a custom regular expression that
   makes sure that the keyword matches only what it should in that
   particular context. To be able to use this feature, and to fully
   understand the examples in this section, you need to understand at
   least the basics of the regular expression syntax.

이 문제에 대한 해결책은 키워드가 맞춤형(custom) 정규표현식을 사용하는 것이다.
이것은 키워드가 특정 맥락에서 해야하는 것과 일치하는 지 확신한다. 이
기능을 사용하려면, 이 섹션의 예제를 완전히 이해할 수 있도록, 적어도
정규 표현식의 기초 문법은 이해해야 한다.

..
   A custom embedded argument regular expression is defined after the
   base name of the argument so that the argument and the regexp are
   separated with a colon. For example, an argument that should match
   only numbers can be defined like `${arg:\d+}`. Using custom
   regular expressions is illustrated by the examples below.

맞춤형 임베디드 인자 정규 표현식은 전달인자의 기본이름 이후에
정의한다. 따라서 인자와 정규 표현식은 콜론으로 구분한다. 예를 들면,
숫자만 일치하는 인자는 `${arg:\d+}` 처럼 정의 할 수 있다. 맞춤형 정규
표현식을 사용하는 것은 아래의 예에서 보여준다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       I execute "ls"
       I execute "ls" with "-lh"
       I type 1 + 2
       I type 53 - 11
       Today is 2011-06-27

   *** Keywords ***
   I execute "${cmd:[^"]+}"
       Run Process    ${cmd}    shell=True

   I execute "${cmd}" with "${opts}"
       Run Process    ${cmd} ${opts}    shell=True

   I type ${a:\d+} ${operator:[+-]} ${b:\d+}
       Calculate    ${a}    ${operator}    ${b}

   Today is ${date:\d{4\}-\d{2\}-\d{2\}}
       Log    ${date}

..
   In the above example keyword :name:`I execute "ls" with "-lh"` matches
   only :name:`I execute "${cmd}" with "${opts}"`. That is guaranteed
   because the custom regular expression `[^"]+` in :name:`I execute
   "${cmd:[^"]}"` means that a matching argument cannot contain any
   quotes. In this case there is no need to add custom regexps to the
   other :name:`I execute` variant.

위의 예에서 :name:`I execute "ls" with "-lh"` 는 :name:`I execute
"${cmd}" with "${opts}"` 에만 일치한다. :name:`I execute
"${cmd:[^"]}"` 의 맞춤형 정규 표현식 `[^"]+` 은 인용부호를 포함하지
않는 인수와 일치하는 것을 의미한다. 이 경우 다른 :name:`I execute`
변이체(variant)에 대한 맞춤형 정규표현식을 추가할 필요가 없다.

..
   .. tip:: If you quote arguments, using regular expression `[^"]+`
	    guarantees that the argument matches only until the first
	    closing quote.

.. tip:: 인용부호를 인수로 사용할 경우, 정규 표현식 `[^"]+` 은
         전달인자와 첫번째 닫는 인용부호까지 일치하는지를 보장한다.

Supported regular expression syntax
'''''''''''''''''''''''''''''''''''

..
   Being implemented with Python, Robot Framework naturally uses Python's
   :name:`re` module that has pretty standard `regular expressions
   syntax`__. This syntax is otherwise fully supported with embedded
   arguments, but regexp extensions in format `(?...)` cannot be
   used. Notice also that matching embedded arguments is done
   case-insensitively. If the regular expression syntax is invalid,
   creating the keyword fails with an error visible in `test execution
   errors`__.

파이썬으로 구현된 Robot Framework는 표준 `정규 표현식 문법`__ 을
지원하기 위해 파이썬의 :name:`re` 모듈을 사용한다. 이 syntax는
임베디드 인자를 완전히 지원하지만 정규 표현식 확장 형식 `(?...)` 은
사용할 수 없다. 임베디드 인자의 일치는 대소문자를 구분하지 않는다.
정규 표현식 구문이 유효하지 않은 경우, 키워드 생성은 `테스트 실행
에러`__ 에서 에러를 표시하고 실패한다.


Escaping special characters
'''''''''''''''''''''''''''

..
   There are some special characters that need to be escaped when used in
   the custom embedded arguments regexp. First of all, possible closing
   curly braces (`}`) in the pattern need to be escaped with a single backslash
   (`\}`) because otherwise the argument would end already there. This is
   illustrated in the previous example with keyword
   :name:`Today is ${date:\\d{4\\}-\\d{2\\}-\\d{2\\}}`.

몇몇 특수 문자는 맞춤형 임베디드 인자 정규 표현식에 사용할 때
이스케이프해야 한다. 가장 먼저 패턴에서 가능한 닫는 중괄호(`}`)는 단일
백슬래쉬(`\}`)로 이스케이프해야 한다. 그렇지 않으면 인자는 이미
종료하기 때문이다. 이것은 이미 이전 예제의 :name:`Today is
${date:\\d{4\\}-\\d{2\\}-\\d{2\\}}` 키워드에서 보여줬다.
      
..
   Backslash (:codesc:`\\`) is a special character in Python regular
   expression syntax and thus needs to be escaped if you want to have a
   literal backslash character. The safest escape sequence in this case
   is four backslashes (`\\\\`) but, depending on the next
   character, also two backslashes may be enough.

백슬래시(:codesc:`\\`)는 파이썬 정규 표현식 문법에서 특수 문자며,
따라서 문자 그대로의 백슬래시 문자를 사용하고 싶다면 이스케이프 해야
한다. 이 경우 가장 안전한 이스케이프 시퀀스는 네 개의
백슬래쉬(`\\\\`) 이지만 다음 문자에 따라 두개의 백슬래시로 충분할 수도
있다.

..
   Notice also that keyword names and possible embedded arguments in them
   should *not* be escaped using the normal `test data escaping
   rules`__. This means that, for example, backslashes in expressions
   like `${name:\w+}` should not be escaped.

키워드 이름과 키워드 내의 임베디드 인자는 보통의 `test data escaping
rules`__ 을 사용하여 이스케이프 하지 *않는다*. 이것은 예를 들어
`${name:\w+}` 같은 식 안에서 백슬래시는 이스케이프 되지 않아야 함을
뜻한다.


Using variables with custom embedded argument regular expressions
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

..
   Whenever custom embedded argument regular expressions are used, Robot
   Framework automatically enhances the specified regexps so that they
   match variables in addition to the text matching the pattern. This
   means that it is always possible to use variables with keywords having
   embedded arguments. For example, the following test case would pass
   using the keywords from the earlier example.

Robot Framework는 맞춤형 임베디드 인자 정규 표현식이 사용될때 마다
자동적으로 기술된 정규표현식을 확장해서 텍스트가 패턴과 일치하는지
확인할 뿐만 아니라 변수와 일치하는지 확인한다. 이것은 항상 임베디드
인수를 가지는 키워드에 변수를 사용할 수 있다는 것을 의미한다. 예를
들어, 다음 test case는 이전 예제의 키워드를 사용하여 패쓰할 수 있다.

.. sourcecode:: robotframework

   *** Variables ***
   ${DATE}    2011-06-27

   *** Test Cases ***
   Example
       I type ${1} + ${2}
       Today is ${DATE}

..
   A drawback of variables automatically matching custom regular
   expressions is that it is possible that the value the keyword gets
   does not actually match the specified regexp. For example, variable
   `${DATE}` in the above example could contain any value and
   :name:`Today is ${DATE}` would still match the same keyword.

자동적으로 맞춤형 정규 표현식에 일치하는 변수의 단점은 키워드가 가져온
값이 실제 기술된 정규표현식과 일치하지 않을 수도 있다는 것이다. 예를
들어, 위의 예제에서 변수 `${DATE}` 는 임의의 값을 포함할 수 있으며,
:name:`Today is ${DATE}` 키워드는 여전히 동일한 키워드와 일치할
것이다.

__ http://en.wikipedia.org/wiki/Regular_expression
__ `Embedded arguments matching too much`_
__ https://docs.python.org/2/library/re.html
__ `Errors and warnings during execution`_
__ Escaping_

Behavior-driven development example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   The biggest benefit of having arguments as part of the keyword name is that it
   makes it easier to use higher-level sentence-like keywords when writing test
   cases in `behavior-driven style`_. The example below illustrates this. Notice
   also that prefixes :name:`Given`, :name:`When` and :name:`Then` are `left out
   of the keyword definitions`__.

키워드 이름의 일부로 전달인자를 가지는 가장 큰 이점은 `behavior-driven
style`_ 로 test case를 적을 때 higher-level sentence-like 키워드를
사용하기가 쉽다는 것이다. 아래 예제가 이것을 보여준다. 접두어
:name:`Given`, :name:`When` 와 :name:`Then` 는 `키워드 정의에서
제외`__ 하고 인식한다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Add two numbers
       Given I have Calculator open
       When I add 2 and 40
       Then result should be 42

   Add negative numbers
       Given I have Calculator open
       When I add 1 and -2
       Then result should be -1

   *** Keywords ***
   I have ${program} open
       Start Program    ${program}

   I add ${number 1} and ${number 2}
       Input Number    ${number 1}
       Push Button     +
       Input Number    ${number 2}
       Push Button     =

   Result should be ${expected}
       ${result} =    Get Result
       Should Be Equal    ${result}    ${expected}

..
   .. note:: Embedded arguments feature in Robot Framework is inspired by
	     how *step definitions* are created in a popular BDD tool Cucumber__.

.. note:: Robot Framework의 임베디드 인자를 사용하는 기능은 인기 BDD
          툴인 Cucumber__ 에서 *step definitions* 를 생성하는 방법에서
          영감을 얻었다.
	  
__ `Ignoring Given/When/Then/And/But prefixes`_
__ http://cukes.info

User keyword return values
--------------------------

..
   Similarly as library keywords, also user keywords can return
   values. Typically return values are defined with the :setting:`[Return]`
   setting, but it is also possible to use BuiltIn_ keywords
   :name:`Return From Keyword` and :name:`Return From Keyword If`.
   Regardless how values are returned, they can be `assigned to variables`__
   in test cases and in other user keywords.

라이브러리 키워드와 비슷하게 user keywords는 값을 리턴할 수 있다. 보통
리턴 값은 :setting:`[Return]` 설정에 정의하지만 BuiltIn_ 키워드
:name:`Return From Keyword` 와 :name:`Return From Keyword If` 를
사용할 수도 있다. 값을 리턴하는 방법에 상관없이 test cases와 다른 user
keywords에서 리턴값은 `변수에 할당할 수 있다`__.

__ `Return values from keywords`_

Using :setting:`[Return]` setting
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   The most common case is that  a user keyword returns one value and it is
   assigned to a scalar variable. When using the :setting:`[Return]` setting, this is
   done by having the return value in the next cell after the setting.

가장 일반적인 경우 user keyword는 한가지 값을 리턴해서 스칼라 변수에
할당한다. :setting:`[Return]` 설정 다음 셀에 리턴 값을 적는다.

..
   User keywords can also return several values, which can then be assigned into
   several scalar variables at once, to a list variable, or to scalar variables
   and a list variable. Several values can be returned simply by
   specifying those values in different cells after the :setting:`[Return]` setting.

또한 User keywords는 여러 값을 리턴할 수도 있다. 이것은 한번에 리스트
변수에 여러 스칼라 변수를 할당 할 수도 있고, 스칼라 변수들에 리스트
변수를 할당할 수도 있다. 여러 값은 :setting:`[Return]` 설정 다음
다른 셀들에 이러한 값을 적으면 된다.

.. sourcecode:: robotframework

   *** Test Cases ***
   One Return Value
       ${ret} =    Return One Value    argument
       Some Keyword    ${ret}

   Multiple Values
       ${a}    ${b}    ${c} =    Return Three Values
       @{list} =    Return Three Values
       ${scalar}    @{rest} =    Return Three Values

   *** Keywords ***
   Return One Value
       [Arguments]    ${arg}
       Do Something    ${arg}
       ${value} =    Get Some Value
       [Return]    ${value}

   Return Three Values
       [Return]    foo    bar    zap

Using special keywords to return
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   BuiltIn_ keywords :name:`Return From Keyword` and :name:`Return From Keyword If`
   allow returning from a user keyword conditionally in the middle of the keyword.
   Both of them also accept optional return values that are handled exactly like
   with the :setting:`[Return]` setting discussed above.

BuiltIn_ 키워드 :name:`Return From Keyword` 와 :name:`Return From
Keyword If` 를 사용하여 user keyword의 중간에서 조건부로 리턴할 수
있다. 두 키워드 모두 위에서 설명한 :setting:`[Return]` 설정과 같이
처리 옵션 리턴 값을 허용한다.
   
..
   The first example below is functionally identical to the previous
   :setting:`[Return]` setting example. The second, and more advanced, example
   demonstrates returning conditionally inside a `for loop`_.

아래의 첫번째 예는 이전의 :setting:`[Return]` 설정 예제와 기능적으로
동일하다. 두번째 더욱 진보된 예는 `for loop`_ 내부에 조건부 반환을
보여준다.
	    
.. sourcecode:: robotframework

   *** Test Cases ***
   One Return Value
       ${ret} =    Return One Value  argument
       Some Keyword    ${ret}

   Advanced
       @{list} =    Create List    foo    baz
       ${index} =    Find Index    baz    @{list}
       Should Be Equal    ${index}    ${1}
       ${index} =    Find Index    non existing    @{list}
       Should Be Equal    ${index}    ${-1}

   *** Keywords ***
   Return One Value
       [Arguments]    ${arg}
       Do Something    ${arg}
       ${value} =    Get Some Value
       Return From Keyword    ${value}
       Fail    This is not executed

   Find Index
       [Arguments]    ${element}    @{items}
       ${index} =    Set Variable    ${0}
       :FOR    ${item}    IN    @{items}
       \    Return From Keyword If    '${item}' == '${element}'    ${index}
       \    ${index} =    Set Variable    ${index + 1}
       Return From Keyword    ${-1}    # Could also use [Return]

..
   .. note:: Both :name:`Return From Keyword` and :name:`Return From Keyword If`
	     are available since Robot Framework 2.8.

.. note:: :name:`Return From Keyword` 와 :name:`Return From Keyword
          If` 는 Robot Framework 2.8부터 사용가능하다.
	     
	     
User keyword teardown
---------------------

..
   User keywords may have a teardown defined using :setting:`[Teardown]` setting.

User keywords는 :setting:`[Teardown]` 설정을 사용할 수 있다.

..
   Keyword teardown works much in the same way as a `test case
   teardown`__.  Most importantly, the teardown is always a single
   keyword, although it can be another user keyword, and it gets executed
   also when the user keyword fails. In addition, all steps of the
   teardown are executed even if one of them fails. However, a failure in
   keyword teardown will fail the test case and subsequent steps in the
   test are not run. The name of the keyword to be executed as a teardown
   can also be a variable.

키워드 teardown은 `test case teardown`__ 과 많은 부분에서 동일하게
동작한다. 가장 중요한 것은 teardown은 비록 다른 user keyword가 될 수도
있고, 그 키워드가 실행하고 실패할 수도 있지만, 항상 단일 키워드이어야
한다. 또한 teardown은 비록 그 절차중 하나가 실패한다고 하더라도 모든
단계를 수행한다. 그러나 키워드 teardown이 실패하면 테스트의 다음
단계는 수행하지 않으며 test case는 실패한다. Teardown으로 실행할
키워드의 이름 역시 변수를 사용할 수 있다.

.. sourcecode:: robotframework

   *** Keywords ***
   With Teardown
       Do Something
       [Teardown]    Log    keyword teardown

   Using variables
       [Documentation]    Teardown given as variable
       Do Something
       [Teardown]    ${TEARDOWN}

__ `test setup and teardown`_
