Modyllic\Loader-- reworked to be essentially a magic filehandle that can
handle our various data sources, eg files, directories of files and DSNs.

Modyllic\Tokenizer-- With dialect specific varieties when needed.  Reworked
to consume incrementally via Modyllic\Loader objects.

Modyllic\Dialect\$DIALECT
    ->parse($loader) returns an array of Modyllic\Command
    ->compile(array of Modyllic\Command) returns string
    ->schema() create and return Modyllic\Schema\$DIALECT object

Modyllic\Command
    With subclasses for each kind of DDL and otherwise that Modyllic
    supports.  Inside these we'd see type objects where type names showed up
    in the SQL, value objects for literals, and other such things for comparable
    content, eg, default charsets.  In addition to standardized properties,
    these objects need to support dialect specific properties.  Generally speaking, if you're generating SQL from a dialect specific property

Modyllic\Schema\$DIALECT
    ->apply( Modyllic\Command )
    ->diff( other Modyllic\Schema\$DIALECT ) -- returns array of Modyllic\Command


modyllic dump
    $dialect = new Modyllic\Dialect\$DIALECT();
    print $dialect->compile($dialect->schema->apply($dialect->parse(new Modyllic\Loader($loaderspec))))

modyllic diff
    $dialect1 = new Modyllic\Generator\$DIALECT1();
    $dialect2 = new Modyllic\Generator\$DIALECT2();
    $schema1 = $dialect1->schema->apply( $dialect1->parse(new Modyllic\Loader($loaderspec1)) );
    $schema2 = $dialect1->schema->apply( $dialect2->parse(new Modyllic\Loader($loaderspec2)) );
    print $dialect1->compile( $schema1->diff($schema2) );
