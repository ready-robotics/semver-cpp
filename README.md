Semvar - The Semantic Versioning [![Build Status](https://travis-ci.org/euskadi31/semver-cpp.png)](https://travis-ci.org/euskadi31/semver-cpp)
================================

semver-cpp implemented the [semver 2.0.0](http://semver.org/spec/v2.0.0.html).

Install
-------

~~~shell
$ mkdir -p build && cd build && cmake .. && make && sudo make install
~~~

Test
----

~~~shell
$ mkdir -p build && cd build && cmake -DBUILD_TESTS:BOOL=True .. && make && make test
~~~

License
-------

semver-cpp is licensed under the MIT licenses.
