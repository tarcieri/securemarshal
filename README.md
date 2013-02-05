SecureMarshal
=============

A Ruby serialization format designed from the ground-up around security.

## Rationale

The recent remote code execution vulnerabilities in many serialization
formats used by Rubyists, including YAML, certain JSON implementations,
and CSV, show us that the serializers for these formats were not written
with security in mind. The same can be said about Ruby's own Marshal format.

Marshal is at least designed to be a serialization format for Ruby objects
exclusively. Serializing arbitrary Ruby objects was tacked onto YAML,
a format originally intended to be handwritten by humans. That said,
Marshal is not well-versioned and is subject to change, and also has a
very chatty method protocol for handling serialization and deserialization.

SecureMarshal aims to solve these problems by creating a format that is
more or less Marshal-compatible (or at least Marshal-inspired) but also
implemented from the ground-up with security in mind. No code from the
existing Marshal implementation is used, nor is SecureMarshal API
compatible with the existing Marshal format.

## Methodology

SecureMarshal takes the following steps to improve security:

* **Only whitelisted classes are allowed**: SecureMarshal will not function
  on any class which is not contained in the whitelist. Classes which can
  be serialized must be explicitly added to the whitelist.

* **No methods are ever called during serialization**: YAML resulted in an
  RCE because certain objects would string eval during a method that Psych
  would call on them during the deserialization process. SecureMarshal
  uses native extensions (a C extension for CRubies/rbx and a Java extension
  on JRuby) to bypass the Ruby method protocol entirely, ensuring that
  attackers cannot execute code as part of the serialization process.

## Installation

Add this line to your application's Gemfile:

    gem 'securemarshal'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install securemarshal

## Usage

TODO: Write usage instructions here

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
