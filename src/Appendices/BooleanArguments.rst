Boolean arguments
=================

..
   Many keywords in Robot Framework `standard libraries`_ accept arguments that
   are handled as Boolean values true or false. If such an argument is given as
   a string, it is considered false if it is either empty or case-insensitively
   equal to `false` or `no`. Other strings are considered true regardless
   their value, and other argument types are tested using same `rules as in Python
   <http://docs.python.org/2/library/stdtypes.html#truth-value-testing>`__.

Robot Framework `표준 라이브러리 <standard libraries_>`__ 의 많은
키워드는 인자를 받는다. 이 인자는 Boolean values 으로 true 또는
false로 처리됩니다. 이러한 인자가 문자열로 주어진다면 이것이
비어있거나 대소문자 관계없이 `false` 또는 `no` 로 되어 있으면,
거짓으로 간주된다. 다른 문자열은 그들의 값과 관계없이 사실로 간주하고,
다른 인자 유형은 `파이썬과 같은 규칙
<http://docs.python.org/2/library/stdtypes.html#truth-value-testing>`__
을 써서 시험된다.


..
   Keyword can also accept other special strings than `false` and `no` that are
   to be considered false. For example, BuiltIn_ keyword `Should Be True` used
   in the examples below considers string `no values` given to its `values`
   argument as false.


키워드는 또 `false` 와 `no` 외의 거짓으로 고려되어야 할 다른 특별한
문자열을 받아 들일 수 있다. 예를 들어, 아래 예에 쓰인 BuiltIn_ 키워드
`Should Be True` 는 `values` 인자로 주어진 `no values` 문자열을
거짓으로 간주된다.


.. sourcecode:: robotframework

   *** Keywords ***
   True examples
       Should Be Equal    ${x}    ${y}    Custom error    values=True         # Strings are generally true.
       Should Be Equal    ${x}    ${y}    Custom error    values=yes          # Same as the above.
       Should Be Equal    ${x}    ${y}    Custom error    values=${TRUE}      # Python `True` is true.
       Should Be Equal    ${x}    ${y}    Custom error    values=${42}        # Numbers other than 0 are true.

   False examples
       Should Be Equal    ${x}    ${y}    Custom error    values=False        # String `false` is false.
       Should Be Equal    ${x}    ${y}    Custom error    values=no           # Also string `no` is false.
       Should Be Equal    ${x}    ${y}    Custom error    values=${EMPTY}     # Empty string is false.
       Should Be Equal    ${x}    ${y}    Custom error    values=${FALSE}     # Python `False` is false.
       Should Be Equal    ${x}    ${y}    Custom error    values=no values    # Special false string in this context.

..
   Note that prior to Robot Framework 2.9 handling Boolean arguments was
   inconsistent. Some keywords followed the above rules, but others simply
   considered all non-empty strings, including `false` and `no`, to be true.

Robot Framework 2.9 이전에 Boolean 인자를 다루는 것은 일관성이 없었다.
일부 키워드는 위의 규칙을 따랐고, 일부는 간단하게 `false` and `no` 를
포함하고 있는, true가 될, all non-empty 문자열로 여겨졌다.

