# elasticsearch-tools

A collection of elasticsearch command line tools for doing things like bulk importing/exporting
and exporting/importing mappings.

It was created because some of the existing import/export tools ran too slow on my machine. Using
the new bulk API seemed to speed things up dramatically.  The other tools I used also weren't
exporting _parent and _routing fields.


## Installation

```bash
npm install -g elasticsearch-tools
```

After installing, you will have access to the following command line tools:

#### Exporting
- es-export-bulk
- es-export-mappings
- es-export-settings
- es-export-aliases

#### Importing
- es-import-bulk
- es-import-mappings
- es-import-settings
- es-import-aliases


## Usage: es-export-bulk

### Options

```bash
es-export-bulk --help

Usage: es-export-bulk [options]

  Options:

    -h, --help                           output usage information
    -v, --version                        output the version number
    -u, --url <url>                      the elasticsearch url to connect to
    -f, --file <file>                    the file to write data to
    --index <index>                      ES OPTION: a comma-separated list of index names to search; use _all or empty string to perform the operation on all indices
    --type <type>                        ES OPTION: a comma-separated list of document types to search; leave empty to perform the operation on all types
    --body <body>                        ES OPTION: the body to send along with this request.
    --analyzer <analyzer>                ES OPTION: The analyzer to use for the query string
    --analyzeWildcard <analyzeWildcard>  ES OPTION: specify whether wildcard and prefix queries should be analyzed (default: false)
    --fields <fields>                    ES OPTION: a comma-separated list of fields to return as part of a hit (default: "*")
    --from <from>                        ES OPTION: starting offset (default: 0)
    --q <q>                              ES OPTION: query in the Lucene query string syntax
    --routing <routing>                  ES OPTION: a comma-separated list of specific routing values
    --scroll <scroll>                    ES OPTION: specify how long a consistent view of the index should be maintained for scrolled search (default: 1m)
    --size <size>                        ES OPTION: number of hits to return
    --sort <sort>                        ES OPTION: a comma-separated list of <field>:<direction> pairs
    --timeout <timeout>                  ES OPTION: explicit operation timeout
```

### Examples

#### export 1 hour of data from local db
```bash
es-export-bulk --url http://localhost:9200 --file ~/backups/elasticsearch/prod/rafflev1.json --index rafflev1 --body '
{"query":{"range":{"timestamp":{"gte":"2014-08-13T11:00:00.000Z","lte":"2014-08-13T12:00:00.000Z"}}}}
'
```


## Usage: es-export-mappings

### Options

```bash
es-export-mappings --help

Usage: es-export-mappings [options]

  Options:

    -h, --help                               output usage information
    -v, --version                            output the version number
    -u, --url <url>                          the elasticsearch url to connect to
    -f, --file <file>                        the file to write data to
    --index <index>                          ES OPTION: String, String[], Boolean — A comma-separated list of index names
    --type <type>                            ES OPTION: String, String[], Boolean — A comma-separated list of document types
    --ignoreUnavailable <ignoreUnavailable>  ES OPTION: Boolean — Whether specified concrete indices should be ignored when unavailable (missing or closed)
    --allowNoIndices <allowNoIndices>        ES OPTION: Boolean — Whether to ignore if a wildcard indices expression resolves into no concrete indices. (This includes _all string or when no indices have been specified)
    --expandWildcards <expandWildcards>      ES OPTION: String — Whether to expand wildcard expression to concrete indices that are open, closed or both.
    --local <local>                          ES OPTION: Boolean — Return local information, do not retrieve the state from master node (default: false)
```

### Examples

#### export mappings from local db
```bash
es-export-mappings --url http://localhost:9200 --file ~/backups/elasticsearch/prod/prod.mappings.json
```


## Usage: es-export-settings

### Options

```bash
es-export-settings --help

Usage: es-export-settings [options]

  Options:

    -h, --help                               output usage information
    -v, --version                            output the version number
    -u, --url <url>                          the elasticsearch url to connect to
    -f, --file <file>                        the file to write data to
    --index <index>                          ES OPTION: String, String[], Boolean — A comma-separated list of index names
    --ignoreUnavailable <ignoreUnavailable>  ES OPTION: Boolean — Whether specified concrete indices should be ignored when unavailable (missing or closed)
    --allowNoIndices <allowNoIndices>        ES OPTION: Boolean — Whether to ignore if a wildcard indices expression resolves into no concrete indices. (This includes _all string or when no indices have been specified)
    --expandWildcards <expandWildcards>      ES OPTION: String — Whether to expand wildcard expression to concrete indices that are open, closed or both.
    --local <local>                          ES OPTION: Boolean — Return local information, do not retrieve the state from master node (default: false)
    --name <name>                            ES OPTION: String, String[], Boolean — The name of the settings that should be included
```

### Examples

#### export settings from local db
```bash
es-export-settings --url http://localhost:9200 --file ~/backups/elasticsearch/prod/prod.settings.json
```


## Usage: es-export-aliases

### Options

```bash
es-export-aliases --help

Usage: es-export-aliases [options]

  Options:

    -h, --help         output usage information
    -v, --version      output the version number
    -u, --url <url>    the elasticsearch url to connect to
    -f, --file <file>  the file to write data to
    --index <index>    ES OPTION: String, String[], Boolean — A comma-separated list of index names
    --local <local>    ES OPTION: Boolean — Return local information, do not retrieve the state from master node (default: false)
    --name <name>      ES OPTION: String, String[], Boolean — The name of the settings that should be included
```

### Examples

#### export aliases from local db
```bash
es-export-aliases --url http://localhost:9200 --file ~/backups/elasticsearch/prod/prod.aliases.json
```


## Usage: es-import-bulk

### Options

```bash
es-import-bulk --help

Usage: es-import-bulk [options]

  Options:

    -h, --help         output usage information
    -v, --version      output the version number
    -u, --url <url>    the elasticsearch url to connect to
    -f, --file <file>  the file to write data to
    -m, --max <items>  the max number of lines to process per batch
```

### Examples

#### import data to local db from file
```bash
es-import-bulk --url http://localhost:9200 --file ~/backups/elasticsearch/prod/rafflev1.json
```


## Usage: es-import-mappings

### Options

```bash
es-import-mappings --help

Usage: es-import-mappings [options]

  Options:

    -h, --help                               output usage information
    -v, --version                            output the version number
    -u, --url <url>                          the elasticsearch url to connect to
    -f, --file <file>                        the file to write data to
    --ignoreConflicts <ignoreConflicts>      ES OPTION: Boolean — Specify whether to ignore conflicts while updating the mapping (default: false)
    --timeout <timeout>                      ES OPTION: Date, Number — Explicit operation timeout
    --masterTimeout <masterTimeout>          ES OPTION: Date, Number — Specify timeout for connection to master
    --ignoreUnavailable <ignoreUnavailable>  ES OPTION: Boolean — Whether specified concrete indices should be ignored when unavailable (missing or closed)
    --allowNoIndices <allowNoIndices>        ES OPTION: Boolean — Whether to ignore if a wildcard indices expression resolves into no concrete indices. (This includes _all string or when no indices have been specified)
    --expandWildcards <expandWildcards>      ES OPTION: String — Whether to expand wildcard expression to concrete indices that are open, closed or both.
```

### Examples

#### import mappings to local db
```bash
es-import-mappings --url http://localhost:9200 --file ~/backups/elasticsearch/prod/prod.mappings.json
```


## Usage: es-import-settings

### Options

```bash
es-import-settings --help

Usage: es-import-settings [options]

  Options:

    -h, --help                               output usage information
    -v, --version                            output the version number
    -u, --url <url>                          the elasticsearch url to connect to
    -f, --file <file>                        the file to write data to
    --masterTimeout <masterTimeout>          ES OPTION: Date, Number — Specify timeout for connection to master
    --ignoreUnavailable <ignoreUnavailable>  ES OPTION: Boolean — Whether specified concrete indices should be ignored when unavailable (missing or closed)
    --allowNoIndices <allowNoIndices>        ES OPTION: Boolean — Whether to ignore if a wildcard indices expression resolves into no concrete indices. (This includes _all string or when no indices have been specified)
    --expandWildcards <expandWildcards>      ES OPTION: String — Whether to expand wildcard expression to concrete indices that are open, closed or both.
```

### Examples

#### import settings to local db
```bash
es-import-settings --url http://localhost:9200 --file ~/backups/elasticsearch/prod/prod.settings.json
```


## Usage: es-import-aliases

### Options

```bash
es-import-aliases --help

Usage: es-import-aliases [options]

  Options:

    -h, --help                       output usage information
    -v, --version                    output the version number
    -u, --url <url>                  the elasticsearch url to connect to
    -f, --file <file>                the file to write data to
    --timeout <timeout>              ES OPTION: Date, Number — Explicit operation timeout
    --masterTimeout <masterTimeout>  ES OPTION: Date, Number — Specify timeout for connection to master
```

### Examples

#### import aliases to local db
```bash
es-import-aliases --url http://localhost:9200 --file ~/backups/elasticsearch/prod/prod.aliases.json
```


## Other Elasticsearch Tools

#### Imports / Exports
- [elasticdump](https://github.com/taskrabbit/elasticsearch-dump)
- [elasticsearch-exporter](https://github.com/mallocator/Elasticsearch-Exporter)


## License

Copyright (c) 2014 skratchdot  
Licensed under the MIT license.
