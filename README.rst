|buildstatus|_
|coverage|_

About
=====

A Python package for ASN.1 parsing, encoding and decoding.

This project is under development and does only support a subset of
the ASN.1 specification syntax. BER, DER, PER and UPER codecs are also
under development.

Project homepage: https://github.com/eerimoq/asn1tools

Documentation: http://asn1tools.readthedocs.org/en/latest

Installation
============

.. code-block:: python

    pip install asn1tools

Example Usage
=============

This is an example ASN.1 specification defining the messages of a
fictitious Foo protocol (based on the FooProtocol on Wikipedia).

.. code-block:: text

   Foo DEFINITIONS ::= BEGIN

       Question ::= SEQUENCE {
           id        INTEGER,
           question  IA5String
       }

       Answer ::= SEQUENCE {
           id        INTEGER,
           answer    BOOLEAN
       }

   END

Scripting
---------

`Compile`_ the ASN.1 specification, and `encode`_ and `decode`_ a
question using the default codec (BER).

.. code-block:: python

   >>> import asn1tools
   >>> foo = asn1tools.compile_file('tests/files/foo.asn')
   >>> encoded = foo.encode('Question', {'id': 1, 'question': 'Is 1+1=3?'})
   >>> encoded
   bytearray(b'0\x0e\x02\x01\x01\x16\x09Is 1+1=3?')
   >>> foo.decode('Question', encoded)
   {'id': 1, 'question': 'Is 1+1=3?'}

The same ASN.1 specification, but using the PER codec.

.. code-block:: python

   >>> import asn1tools
   >>> foo = asn1tools.compile_file('tests/files/foo.asn', 'per')
   >>> encoded = foo.encode('Question', {'id': 1, 'question': 'Is 1+1=3?'})
   >>> encoded
   bytearray(b'\x01\x01\tIs 1+1=3?')
   >>> foo.decode('Question', encoded)
   {'id': 1, 'question': 'Is 1+1=3?'}

See the `examples`_ folder for additional examples.

Command line tool
-----------------

Decode given encoded Question using the default codec (BER).

.. code-block:: text

   $ asn1tools decode tests/files/foo.asn Question 300e0201011609497320312b313d333f
   id: 1
   question: Is 1+1=3?
   $

Decode given encoded Question using the UPER codec.

.. code-block:: text

   $ asn1tools decode --codec uper tests/files/foo.asn Question 01010993cd03156c5eb37e
   id: 1
   question: Is 1+1=3?
   $

Contributing
============

#. Fork the repository.

#. Implement the new feature or bug fix.

#. Implement test case(s) to ensure that future changes do not break
   legacy.

#. Run the tests.

   .. code-block:: text

      make test

#. Create a pull request.

.. |buildstatus| image:: https://travis-ci.org/eerimoq/asn1tools.svg?branch=master
.. _buildstatus: https://travis-ci.org/eerimoq/asn1tools

.. |coverage| image:: https://coveralls.io/repos/github/eerimoq/asn1tools/badge.svg?branch=master
.. _coverage: https://coveralls.io/github/eerimoq/asn1tools

.. _Compile: http://asn1tools.readthedocs.io/en/latest/#asn1tools.compile_file
.. _encode: http://asn1tools.readthedocs.io/en/latest/#asn1tools.compiler.Specification.encode
.. _decode: http://asn1tools.readthedocs.io/en/latest/#asn1tools.compiler.Specification.decode
.. _examples: https://github.com/eerimoq/asn1tools/tree/master/examples
