Using listener interface
========================

..
   Robot Framework has a listener interface that can be used to receive
   notifications about test execution. Listeners are classes or modules
   with certain special methods, and they can be implemented both with
   Python and Java. Example uses of the listener interface include
   external test monitors, sending a mail message when a test fails, and
   communicating with other systems.

로봇 프레임워크는 테스트 수행에 대한 알림을 받아들이는데에 사용되는
리스너 인터페이스를 가지고 있다. 리스너는 Python과 Java에서 동작
가능한 특별한 메쏘드를 가진 클래스 혹은 모듈이다. 리스너 인터페이스의
예제 사용은 외부 테스트 모니터를 포함하고, 테스트가 실패할 경우 메일을
보내주고, 다른 시스템과 통신한다.

.. contents::
   :depth: 2
   :local:

Taking listeners into use
-------------------------

..
   Listeners are taken into use from the command line with the :option:`--listener`
   option, so that the name of the listener is given to it as an argument. The
   listener name is got from the name of the class or module implementing the
   listener interface, similarly as `test library names`_ are got from classes
   implementing them. The specified listeners must be in the same `module search
   path`_ where test libraries are searched from when they are imported. Other
   option is to give an absolute or a relative path to the listener file
   `similarly as with test libraries`__. It is possible to take multiple listeners
   into use by using this option several times

리스너는 :option:`--listener` 옵션을 가지고 커맨드 라인에서 사용할 수
있다. 리스너의 이름은 전달인자로 주어진다. 리스너 이름은 `test library
names`_ 가 수행되는 클래스에서 이름을 따온것과 비슷하게, 리스너
인터페이스를 수행하는 클래스나 모듈의 이름에서 차용한다. 정확한
리스너는 임포트 된 곳에서 라이브러리를 탐색하는, 같은 `module search
path`_ 안에 있어야만 한다. 다른 옵션은 절대적이거나 상대적인 리스너
파일 경로를 갖는다 `similarly as with test libraries`__ . 이 옵션을
여러번 사용해서 여러개의 리스너를 사용할 수 있다::

   robot --listener MyListener tests.robot
   robot --listener com.company.package.Listener tests.robot
   robot --listener path/to/MyListener.py tests.robot
   robot --listener module.Listener --listener AnotherListener tests.robot

..
   It is also possible to give arguments to listener classes from the command
   line. Arguments are specified after the listener name (or path) using a colon
   (`:`) as a separator. If a listener is given as an absolute Windows path,
   the colon after the drive letter is not considered a separator. Starting from
   Robot Framework 2.8.7, it is possible to use a semicolon (`;`) as an
   alternative argument separator. This is useful if listener arguments
   themselves contain colons, but requires surrounding the whole value with
   quotes on UNIX-like operating systems

커맨드 라인에서 리스너 클래스에 전달인자를 제공할 수 있다. 전달인자는
전달인자 이름(혹은 경로) 뒤에 콜론(`:`)을 구분자로 하여 그 뒤에
명시된다. 만일 리스너가 윈도우즈 절대 경로를 가진다면, 드라이브 레터
뒤의 콜론은 구분자로 여겨지지 않는다. 로봇 프레임워크 2.8.7부터,
세미콜론(`;`)이 대체 전달인자 구분자로 사용된다. 리스너 전달인자가
콜론을 포함했을 때 유용하다. 하지만 유닉스 계열 운영체제에서는 전체
값을 따옴표로 둘러쌀 필요가 있다::

   robot --listener listener.py:arg1:arg2 tests.robot
   robot --listener "listener.py;arg:with:colons" tests.robot
   robot --listener C:\Path\Listener.py;D:\data;E:\extra tests.robot

__ `Using physical path to library`_

Available listener interface methods
------------------------------------

..
   Robot Framework creates an instance of the listener class with given arguments
   when test execution starts. During the test execution, Robot Framework calls
   listeners' methods when test suites, test cases and keywords start and end. It
   also calls the appropriate methods when output files are ready, and finally at
   the end it calls the `close` method. A listener is not required to
   implement any official interface, and it only needs to have the methods it
   actually needs.

로봇 프레임워크는 수행이 시작될 때 주어진 전달인자의 리스너 클래스
인스턴스를 생성한다. 테스트를 수행할 때, 로봇 프레임워크는 리스너의
메쏘드를 호출한다. 아웃풋 파일이 준비되었을 때, 적절한 메쏘드를
호출한다. 그리고 마지막으로 `close` 메쏘드를 호출한다. 리스너는
공식적인 인터페이스에서는 수행될 필요가 없다. 실제 필요한 메쏘드만
가진다.

Listener interface versions
~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   The signatures of methods related to test execution progress were changed in
   Robot Framework 2.1. This change was made to allow new information to be added
   to the listener interface without breaking existing listeners.
   A listener must have attribute `ROBOT_LISTENER_API_VERSION` with value 2,
   either as a string or as an integer, to be recognized as a listener.
   The old listener interface has been removed in Robot Framework 3.0.

테스트 수행 프로그레스에 관련된 메소드의 특징은 로봇 프레임워크
2.1부터 바꼈다. 이러한 변화는 새로운 정보를 리스너 인터페이스에
존재하는 리스너를 파괴하지 않고 추가할 수 있도록 한다. 리스너는
`ROBOT_LISTENER_API_VERSION` 값 2를 스트링이나 정수로 가지는 속성을
리스너로서 인지하기 위해 가져야 한다. 오래된 리스너 인터페이스는 로봇
프레임워크 3.0부터는 삭제되었다.

Listener interface method signatures
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   All listener methods related to test execution progress have the same
   signature `method(name, attributes)`, where `attributes`
   is a dictionary containing details of the event. The following table
   lists all the available methods in the listener interface and the
   contents of the `attributes` dictionary, where applicable. Keys
   of the dictionary are strings. All of these methods have also
   `camelCase` aliases.  Thus, for example, `startSuite` is a
   synonym to `start_suite`.

모든 수행 프로그래스에 관련된 리스너 메소드는 같은 특징 `method(name,
attributes)` 를 갖는다. `attributes` 는 이벤트에 대해 자세한 내용을
포함하는 딕셔너리이다. 아래 표에 리스너 인터페이스에서 사용가능한 모든
메쏘드와 `attributes` 딕셔너리의 컨텐츠를 적용할 수 있도록 나열해
두었다. 딕셔너리의 키는 스트링이다. 이 메소드 모두는 `camelCase` 라는
별명을 갖는다. 그러므로, `startSuite`는 `start_suite` 와 동의어이다.

.. table:: Available methods in the listener interface
   :class: tabular

   +------------------+------------------+----------------------------------------------------------------+
   |    Method        |    Arguments     |                     Attributes / Explanation                   |
   +==================+==================+================================================================+
   | start_suite      | name, attributes | Keys in the attribute dictionary:                              |
   |                  |                  |                                                                |
   |                  |                  | * `id`: Suite id. `s1` for the top level suite, `s1-s1`        |
   |                  |                  |   for its first child suite, `s1-s2` for the second            |
   |                  |                  |   child, and so on. New in RF 2.8.5.                           |
   |                  |                  | * `longname`: Suite name including parent suites.              |
   |                  |                  | * `doc`: Suite documentation.                                  |
   |                  |                  | * `metadata`: `Free test suite metadata`_ as a dictionary/map. |
   |                  |                  | * `source`: An absolute path of the file/directory the suite   |
   |                  |                  |   was created from. New in RF 2.7.                             |
   |                  |                  | * `suites`: Names of the direct child suites this suite has    |
   |                  |                  |   as a list.                                                   |
   |                  |                  | * `tests`: Names of the tests this suite has as a list.        |
   |                  |                  |   Does not include tests of the possible child suites.         |
   |                  |                  | * `totaltests`: The total number of tests in this suite.       |
   |                  |                  |   and all its sub-suites as an integer.                        |
   |                  |                  | * `starttime`: Suite execution start time.                     |
   +------------------+------------------+----------------------------------------------------------------+
   | end_suite        | name, attributes | Keys in the attribute dictionary:                              |
   |                  |                  |                                                                |
   |                  |                  | * `id`: Same as in `start_suite`.                              |
   |                  |                  | * `longname`: Same as in `start_suite`.                        |
   |                  |                  | * `doc`: Same as in `start_suite`.                             |
   |                  |                  | * `metadata`: Same as in `start_suite`.                        |
   |                  |                  | * `source`: Same as in `start_suite`.                          |
   |                  |                  | * `starttime`: Same as in `start_suite`.                       |
   |                  |                  | * `endtime`: Suite execution end time.                         |
   |                  |                  | * `elapsedtime`: Total execution time in milliseconds as       |
   |                  |                  |   an integer                                                   |
   |                  |                  | * `status`: Suite status as string `PASS` or `FAIL`.           |
   |                  |                  | * `statistics`: Suite statistics (number of passed             |
   |                  |                  |   and failed tests in the suite) as a string.                  |
   |                  |                  | * `message`: Error message if suite setup or teardown          |
   |                  |                  |   has failed, empty otherwise.                                 |
   +------------------+------------------+----------------------------------------------------------------+
   | start_test       | name, attributes | Keys in the attribute dictionary:                              |
   |                  |                  |                                                                |
   |                  |                  | * `id`: Test id in format like `s1-s2-t2`, where               |
   |                  |                  |   the beginning is the parent suite id and the last part       |
   |                  |                  |   shows test index in that suite. New in RF 2.8.5.             |
   |                  |                  | * `longname`: Test name including parent suites.               |
   |                  |                  | * `doc`: Test documentation.                                   |
   |                  |                  | * `tags`: Test tags as a list of strings.                      |
   |                  |                  | * `critical`: `yes` or `no` depending is test considered       |
   |                  |                  |   critical or not.                                             |
   |                  |                  | * `template`: The name of the template used for the test.      |
   |                  |                  |   An empty string if the test not templated.                   |
   |                  |                  | * `starttime`: Test execution execution start time.            |
   +------------------+------------------+----------------------------------------------------------------+
   | end_test         | name, attributes | Keys in the attribute dictionary:                              |
   |                  |                  |                                                                |
   |                  |                  | * `id`: Same as in `start_test`.                               |
   |                  |                  | * `longname`: Same as in `start_test`.                         |
   |                  |                  | * `doc`: Same as in `start_test`.                              |
   |                  |                  | * `tags`: Same as in `start_test`.                             |
   |                  |                  | * `critical`: Same as in `start_test`.                         |
   |                  |                  | * `template`: Same as in `start_test`.                         |
   |                  |                  | * `starttime`: Same as in `start_test`.                        |
   |                  |                  | * `endtime`: Test execution execution end time.                |
   |                  |                  | * `elapsedtime`: Total execution time in milliseconds as       |
   |                  |                  |   an integer                                                   |
   |                  |                  | * `status`: Test status as string `PASS` or `FAIL`.            |
   |                  |                  | * `message`: Status message. Normally an error                 |
   |                  |                  |   message or an empty string.                                  |
   +------------------+------------------+----------------------------------------------------------------+
   | start_keyword    | name, attributes | `name` is the full keyword name containing                     |
   |                  |                  | possible library or resource name as a prefix.                 |
   |                  |                  | For example, `MyLibrary.Example Keyword`.                      |
   |                  |                  |                                                                |
   |                  |                  | Keys in the attribute dictionary:                              |
   |                  |                  |                                                                |
   |                  |                  | * `type`: String `Keyword` for normal                          |
   |                  |                  |   keywords and `Test Setup`, `Test                             |
   |                  |                  |   Teardown`, `Suite Setup` or `Suite                           |
   |                  |                  |   Teardown` for keywords used in suite/test                    |
   |                  |                  |   setup/teardown.                                              |
   |                  |                  | * `kwname`: Name of the keyword without library or             |
   |                  |                  |   resource prefix. New in RF 2.9.                              |
   |                  |                  | * `libname`: Name of the library or resource the               |
   |                  |                  |   keyword belongs to, or an empty string when                  |
   |                  |                  |   the keyword is in a test case file. New in RF 2.9.           |
   |                  |                  | * `doc`: Keyword documentation.                                |
   |                  |                  | * `args`: Keyword's arguments as a list of strings.            |
   |                  |                  | * `assign`: A list of variable names that keyword's            |
   |                  |                  |   return value is assigned to. New in RF 2.9.                  |
   |                  |                  | * `starttime`: Keyword execution start time.                   |
   +------------------+------------------+----------------------------------------------------------------+
   | end_keyword      | name, attributes | `name` is the full keyword name containing                     |
   |                  |                  | possible library or resource name as a prefix.                 |
   |                  |                  | For example, `MyLibrary.Example Keyword`.                      |
   |                  |                  |                                                                |
   |                  |                  | Keys in the attribute dictionary:                              |
   |                  |                  |                                                                |
   |                  |                  | * `type`: Same as with `start_keyword`.                        |
   |                  |                  | * `kwname`: Same as with `start_keyword`.                      |
   |                  |                  | * `libname`: Same as with `start_keyword`.                     |
   |                  |                  | * `doc`: Same as with `start_keyword`.                         |
   |                  |                  | * `args`: Same as with `start_keyword`.                        |
   |                  |                  | * `assign`: Same as with `start_keyword`.                      |
   |                  |                  | * `starttime`: Same as with `start_keyword`.                   |
   |                  |                  | * `endtime`: Keyword execution end time.                       |
   |                  |                  | * `elapsedtime`: Total execution time in milliseconds as       |
   |                  |                  |   an integer                                                   |
   |                  |                  | * `status`: Keyword status as string `PASS` or `FAIL`.         |
   +------------------+------------------+----------------------------------------------------------------+
   | log_message      | message          | Called when an executed keyword writes a log                   |
   |                  |                  | message. `message` is a dictionary with                        |
   |                  |                  | the following keys:                                            |
   |                  |                  |                                                                |
   |                  |                  | * `message`: The content of the message.                       |
   |                  |                  | * `level`: `Log level`_ used in logging the message.           |
   |                  |                  | * `timestamp`: Message creation time in format                 |
   |                  |                  |   `YYYY-MM-DD hh:mm:ss.mil`.                                   |
   |                  |                  | * `html`: String `yes` or `no` denoting whether the message    |
   |                  |                  |   should be interpreted as HTML or not.                        |
   +------------------+------------------+----------------------------------------------------------------+
   | message          | message          | Called when the framework itself writes a syslog_              |
   |                  |                  | message. `message` is a dictionary with                        |
   |                  |                  | same keys as with `log_message` method.                        |
   +------------------+------------------+----------------------------------------------------------------+
   | library_import   | name, attributes | Called when a library has been imported. `name` is the name of |
   |                  |                  | the imported library. If the library has been imported using   |
   |                  |                  | the `WITH NAME syntax`_, `name` is the specified alias.        |
   |                  |                  |                                                                |
   |                  |                  | Keys in the attribute dictionary:                              |
   |                  |                  |                                                                |
   |                  |                  | * `args`: Arguments passed to the library as a list.           |
   |                  |                  | * `originalname`: The original library name when using the     |
   |                  |                  |   WITH NAME syntax, otherwise same as `name`.                  |
   |                  |                  | * `source`: An absolute path to the library source. `None`     |
   |                  |                  |   with libraries implemented with Java or if getting the       |
   |                  |                  |   source of the library failed for some reason.                |
   |                  |                  | * `importer`: An absolute path to the file importing the       |
   |                  |                  |   library. `None` when BuiltIn_ is imported well as when       |
   |                  |                  |   using the :name:`Import Library` keyword.                    |
   |                  |                  |                                                                |
   |                  |                  | New in Robot Framework 2.9.                                    |
   +------------------+------------------+----------------------------------------------------------------+
   | resource_import  | name, attributes | Called when a resource file has been imported. `name` is       |
   |                  |                  | the name of the imported resource file without the file        |
   |                  |                  | extension.                                                     |
   |                  |                  |                                                                |
   |                  |                  | Keys in the attribute dictionary:                              |
   |                  |                  |                                                                |
   |                  |                  | * `source`: An absolute path to the imported resource file.    |
   |                  |                  | * `importer`: An absolute path to the file importing the       |
   |                  |                  |   resource file. `None` when using the :name:`Import Resource` |
   |                  |                  |   keyword.                                                     |
   |                  |                  |                                                                |
   |                  |                  | New in Robot Framework 2.9.                                    |
   +------------------+------------------+----------------------------------------------------------------+
   | variables_import | name, attributes | Called when a variable file has been imported. `name` is       |
   |                  |                  | the name of the imported variable file with the file           |
   |                  |                  | extension.                                                     |
   |                  |                  |                                                                |
   |                  |                  | Keys in the attribute dictionary:                              |
   |                  |                  |                                                                |
   |                  |                  | * `args`: Arguments passed to the variable file as a list.     |
   |                  |                  | * `source`: An absolute path to the imported variable file.    |
   |                  |                  | * `importer`: An absolute path to the file importing the       |
   |                  |                  |   resource file. `None` when using the :name:`Import           |
   |                  |                  |   Variables` keyword.                                          |
   |                  |                  |                                                                |
   |                  |                  | New in Robot Framework 2.9.                                    |
   +------------------+------------------+----------------------------------------------------------------+
   | output_file      | path             | Called when writing to an output file is                       |
   |                  |                  | finished. The path is an absolute path to the file.            |
   +------------------+------------------+----------------------------------------------------------------+
   | log_file         | path             | Called when writing to a log file is                           |
   |                  |                  | finished. The path is an absolute path to the file.            |
   +------------------+------------------+----------------------------------------------------------------+
   | report_file      | path             | Called when writing to a report file is                        |
   |                  |                  | finished. The path is an absolute path to the file.            |
   +------------------+------------------+----------------------------------------------------------------+
   | debug_file       | path             | Called when writing to a debug file is                         |
   |                  |                  | finished. The path is an absolute path to the file.            |
   +------------------+------------------+----------------------------------------------------------------+
   | close            |                  | Called after all test suites, and test cases in                |
   |                  |                  | them, have been executed. With `library listeners`__ called    |
   |                  |                  | when the library goes out of scope.                            |
   +------------------+------------------+----------------------------------------------------------------+

..
   The available methods and their arguments are also shown in a formal Java
   interface specification below. Contents of the `java.util.Map attributes` are
   as in the table above.  It should be remembered that a listener *does not* need
   to implement any explicit interface or have all these methods.

사용가능한 메소드와 그것의 전달인자는 아래의 형식적인 Java 인터페이스
스펙에 정리하였다. `java.util.Map attributes` 의 내용은 위의 표에
나타나있다. 리스너는 외부의 인터페이스에서 *수행될 필요가 없고* 이
메소드를 모두 갖는다는 것을 명심해야 한다.

.. sourcecode:: java

   public interface RobotListenerInterface {
       public static final int ROBOT_LISTENER_API_VERSION = 2;
       void startSuite(String name, java.util.Map attributes);
       void endSuite(String name, java.util.Map attributes);
       void startTest(String name, java.util.Map attributes);
       void endTest(String name, java.util.Map attributes);
       void startKeyword(String name, java.util.Map attributes);
       void endKeyword(String name, java.util.Map attributes);
       void logMessage(java.util.Map message);
       void message(java.util.Map message);
       void outputFile(String path);
       void logFile(String path);
       void reportFile(String path);
       void debugFile(String path);
       void close();
   }

__ `Test libraries as listeners`_

Listeners logging
-----------------

..
   Robot Framework offers a `programmatic logging APIs`_ that listeners can
   utilize. There are some limitations, however, and how different listener
   methods can log messages is explained in the table below.

로봇 프레임워크는 리스너가 활용할 수 있는 `programmatic logging APIs`_
를 제안한다. 그러나 몇가지 한계는 있다. 리스너 메쏘드가 어떻게 다른
메시지를 남길 수 있는지 아래 표에 정리해 두었다.

.. table:: How listener methods can log
   :class: tabular

   +----------------------+---------------------------------------------------+
   |         Methods      |                   Explanation                     |
   +======================+===================================================+
   | start_keyword,       | Messages are logged to the normal `log file`_     |
   | end_keyword,         | under the executed keyword.                       |
   | log_message          |                                                   |
   +----------------------+---------------------------------------------------+
   | start_suite,         | Messages are logged to the syslog_. Warnings are  |
   | end_suite,           | shown also in the `execution errors`_ section of  |
   | start_test, end_test | the normal log file.                              |
   +----------------------+---------------------------------------------------+
   | message              | Messages are normally logged to the syslog. If    |
   |                      | this method is used while a keyword is executing, |
   |                      | messages are logged to the normal log file.       |
   +----------------------+---------------------------------------------------+
   | Other methods        | Messages are only logged to the syslog.           |
   +----------------------+---------------------------------------------------+

..
   .. note:: To avoid recursion, messages logged by listeners are not sent to
	     listener methods `log_message` and `message`.

.. note:: 반복을 피하기 위해서, 리스너에 의해 남겨진 메시지는 리스너
          메소드 `log_message` 와 `message` 에 보내지 말아야 한다.

Listener examples
-----------------

..
   The first simple example is implemented in a Python module. It mainly
   illustrates that using the listener interface is not very complicated.

첫번째 간단한 예제는 Python 모듈로 수행된 것이다. 주로 리스너
인터페이스를 사용하는 것이 별로 복잡하지 않음을 설명한다.

.. sourcecode:: python

   ROBOT_LISTENER_API_VERSION = 2

   def start_test(name, attrs):
       print 'Executing test %s' % name

   def start_keyword(name, attrs):
       print 'Executing keyword %s with arguments %s' % (name, attrs['args'])

   def log_file(path):
       print 'Test log available at %s' % path

   def close():
       print 'All tests executed'

..
   The second example, which still uses Python, is slightly more complicated. It
   writes all the information it gets into a text file in a temporary directory
   without much formatting. The filename may be given from the command line, but
   also has a default value. Note that in real usage, the `debug file`_
   functionality available through the command line option :option:`--debugfile` is
   probably more useful than this example.

(Python) 두번째 예제는 조금 더 복잡하다. 많은 서식 설정 없이, 일시적인
디렉토리의 텍스트 파일에 모든 정보를 입력한다. 실제 사용에서 `debug
file`_ 기능은 커맨드 라인 옵션 :option:`--debugfile` 을 이용하여
사용가능하다. 실제 사용에서 커맨드 라인 옵션 :option:`--debugfile` 에
의해 이용가능한 `debug file`_ 기능은 이 예제보다 더 유용할 것이다.

.. sourcecode:: python

   import os.path
   import tempfile


   class PythonListener:

       ROBOT_LISTENER_API_VERSION = 2

       def __init__(self, filename='listen.txt'):
           outpath = os.path.join(tempfile.gettempdir(), filename)
           self.outfile = open(outpath, 'w')

       def start_suite(self, name, attrs):
           self.outfile.write("%s '%s'\n" % (name, attrs['doc']))

       def start_test(self, name, attrs):
           tags = ' '.join(attrs['tags'])
           self.outfile.write("- %s '%s' [ %s ] :: " % (name, attrs['doc'], tags))

       def end_test(self, name, attrs):
           if attrs['status'] == 'PASS':
               self.outfile.write('PASS\n')
           else:
               self.outfile.write('FAIL: %s\n' % attrs['message'])

        def end_suite(self, name, attrs):
            self.outfile.write('%s\n%s\n' % (attrs['status'], attrs['message']))

        def close(self):
            self.outfile.close()

..
   The third example implements the same functionality as the previous one, but uses Java instead of Python.

세번째 예제는 이전 것과 같은 기능을 수행한다. Python 대신 Java를
사용하였다.

.. sourcecode:: java

   import java.io.*;
   import java.util.Map;
   import java.util.List;


   public class JavaListener {

       public static final int ROBOT_LISTENER_API_VERSION = 2;
       public static final String DEFAULT_FILENAME = "listen_java.txt";
       private BufferedWriter outfile = null;

       public JavaListener() throws IOException {
           this(DEFAULT_FILENAME);
       }

       public JavaListener(String filename) throws IOException {
           String tmpdir = System.getProperty("java.io.tmpdir");
           String sep = System.getProperty("file.separator");
           String outpath = tmpdir + sep + filename;
           outfile = new BufferedWriter(new FileWriter(outpath));
       }

       public void startSuite(String name, Map attrs) throws IOException {
           outfile.write(name + " '" + attrs.get("doc") + "'\n");
       }

       public void startTest(String name, Map attrs) throws IOException {
           outfile.write("- " + name + " '" + attrs.get("doc") + "' [ ");
           List tags = (List)attrs.get("tags");
           for (int i=0; i < tags.size(); i++) {
              outfile.write(tags.get(i) + " ");
           }
           outfile.write(" ] :: ");
       }

       public void endTest(String name, Map attrs) throws IOException {
           String status = attrs.get("status").toString();
           if (status.equals("PASS")) {
               outfile.write("PASS\n");
           }
           else {
               outfile.write("FAIL: " + attrs.get("message") + "\n");
           }
       }

       public void endSuite(String name, Map attrs) throws IOException {
           outfile.write(attrs.get("status") + "\n" + attrs.get("message") + "\n");
       }

       public void close() throws IOException {
           outfile.close();
       }

   }

Test libraries as listeners
---------------------------

..
   Sometimes it is useful also for `test libraries`_ to get notifications about
   test execution. This allows them, for example, to perform certain clean-up
   activities automatically when a test suite or the whole test execution ends.

때때로 `test libraries`_ 에게 테스트 수행에 관련된 알람을 얻는것은
유용하다. 예를들어,테스트 스위트나 전체 테스트 수행이 끝났을 때 어떤
clean-up 활동을 자동으로 수행할 수 있게 한다.

..
   .. note:: This functionality is new in Robot Framework 2.8.5.

.. note:: 이 기능은 로봇 프레임워크 2.8.5부터 적용되었다.

Registering listener
~~~~~~~~~~~~~~~~~~~~

..
   A test library can register a listener by using `ROBOT_LIBRARY_LISTENER`
   attribute. The value of this attribute should be an instance of the listener
   to use. It may be a totally independent listener or the library itself can
   act as a listener. To avoid listener methods to be exposed as keywords in
   the latter case, it is possible to prefix them with an underscore.
   For example, instead of using `end_suite` or `endSuite`, it is
   possible to use `_end_suite` or `_endSuite`.

테스트 라이브러리는 `ROBOT_LIBRARY_LISTENER` 속성을 이용해서 리스너를
등록할 수 있다. 이 속성의 값은 사용할 리스너의 인스턴스일 것이다. 아마
전체적으로 독립적인 리스너이거나 라이브러리가 리스너의 역할을 할
것이다. 나중의 케이스에서 리스너 메쏘가 키워드로 노출되는 것을 피하기
위해서, 언더바(_)를 말머리로 달 수 있다. 예를들어, `end_suite` 나
`endSuite` 를 사용하는 것 대신에 `_end_suite` 나 `_endSuite` 를 사용할
수 있다.

..
   Following examples illustrates using an external listener as well as library
   acting as a listener itself:

다음 예제는 외부의 리스너와 리스너처럼 작동하는 라이브러리를 사용하는
것을 안내한다:

.. sourcecode:: java

   import my.project.Listener;

   public class JavaLibraryWithExternalListener {
       public static final Listener ROBOT_LIBRARY_LISTENER = new Listener();
       public static final String ROBOT_LIBRARY_SCOPE = "GLOBAL";
       public static final int ROBOT_LISTENER_API_VERSION = 2;

       // actual library code here ...
   }

.. sourcecode:: python

   class PythonLibraryAsListenerItself(object):
       ROBOT_LIBRARY_SCOPE = 'TEST SUITE'
       ROBOT_LISTENER_API_VERSION = 2

       def __init__(self):
           self.ROBOT_LIBRARY_LISTENER = self

       def _end_suite(self, name, attrs):
           print 'Suite %s (%s) ending.' % (name, attrs['id'])

       # actual library code here ...

..
   As the seconds example above already demonstrated, library listeners have to
   specify `listener interface versions`_ using `ROBOT_LISTENER_API_VERSION`
   attribute exactly like any other listener.

위의 두번째 예제는 이미 입증되었듯이, 라이브러리 리스너는 정확히 다른
리스너 처럼 `ROBOT_LISTENER_API_VERSION` 기능을 이용하여 `listener
interface versions`_ 를 명시해야 한다.

..
   Starting from version 2.9, you can also provide any list like object of
   instances in the `ROBOT_LIBRARY_LISTENER` attribute. This will cause all
   instances of the list to be registered as listeners.

버전 2.9 부터, `ROBOT_LIBRARY_LISTENER` 기능에서 인스턴스의 객체 같은
리스트를 제공할 수 있다. 리스트의 모든 인스턴스가 리스너로 등록될 수
있게 한다.

Called listener methods
~~~~~~~~~~~~~~~~~~~~~~~

..
   Library's listener will get notifications about all events in suites where
   the library is imported. In practice this means that `start_suite`,
   `end_suite`, `start_test`, `end_test`, `start_keyword`,
   `end_keyword`, `log_message`, and `message` methods are
   called inside those suites.

라이브러리의 리스너는 라이브러리가 임포팅된 스위트의 모든 이벤트에
관한 알람을 받을 것이다. 실제로 이것은 `start_suite`, `end_suite`,
`start_test`, `end_test`, `start_keyword`, `end_keyword`,
`log_message`, `message` 메쏘드가 스위트 내부에서 호출되어야 함을
의미한다.

..
   If the library creates a new listener instance every time when the library
   itself is instantiated, the actual listener instance to use will change
   according to the `test library scope`_.
   In addition to the previously listed listener methods, `close`
   method is called when the library goes out of the scope.

만약 라이브러리가 예로 들어질때마다 새로운 리스너 인스턴스를 매번
생성한다면, 실제 사용할 리스너 인스턴스는 `test library scope`_ 에
따라 바뀐다. 이전에 나열되었던 리스너 메소드와 더불어, `close`
메쏘드는 라이브러리가 스코프를 벗어날 때 호출된다.

..
   See `Listener interface method signatures`_ section above
   for more information about all these methods.

이 메쏘드에 관해 더 많은 정보를 위해 `Listener interface method
signatures`_ 섹션을 참고하면 된다.
