Jackalope Filesystem PHPCR implementation
=========================================

[![Build Status](https://travis-ci.org/jackalope/jackalope-fs.svg?branch=master)](https://travis-ci.org/jackalope/jackalope-fs)

This is a WIP implementation to support a filesystem implementation of PHPCR.

Connecting
----------

Connect as follows:

    $factory = new RepositoryFactoryFilesystem();
    $repository = $factory->getRepository(array(
        'path' => '/home/mystuff/somefolder',
    ));
    $credentials = new SimpleCredentials('admin', 'admin');
    $session = $repository->login($credentials);

Options:

- **path**: (required) Path to store data, indexes, etc.
- **search.enabled**: If search should be enabled or not (default true)
- **search.zend.hide_destruct_exception**: Hack to hide exceptions from the
  ZendSearch engine when running functional tests.
- **filesystem.adapter**: Filesystem adapter to use, either `local` for using
  the filesystem or `array` to use in-memory storage (for testing)

Limitations
-----------

### Node copy

- References not updated within copied subtree (this test is missing from
  PHPCR-API tests)

### Querying

#### ZendSearch Lucene (native PHP)

Not supported:

- **Node type inheritance**: Currently node type inheritance is not taken into
  account - this should be fixed ASAP
- **Joins**: Will need to be implemented in a post processor
- **LOWERCASE, UPPERCASE, LENGTH operands**: Same as above
- **SQL and XPath query langauges**: Will probably never be implemented
- **Full text search**: Easy to implement if we add an additional search index

### File handling

- Files (binary data in the repository) are not current handled in a memory
  efficient manner. This will be addressed.

Testing
-------

The default ZendSearch implementation doesn't behave very well when the full
test suite is being run - it will tend to become corrupt after a certain
number of operations. It is therefore necessary to batch the tests.
