## Welcome to ElasticSearch Guide

Welcome to the definitive guide of ElasticSearch.
In this page I show the most important things of ElasticSearch.

### Index

The first step to use ElasticSearch is index the documents.

Bulk instance

```markdown
$bulk = [ 'body' => [], ];
$results = [] // DATA TO INDEX

foreach ($results as $key => $result) {
    $bulk['body'][] = [
        'index' => [
            '_index' => $indexName //INDEX NAME,
            '_id' => $id // IDENTIFIER,
        ],
    ];

    $bulk['body'][] = $result;

    if ($key % 1000 === 0) {
        try {
            $this->elasticsearch->bulk($bulk);
        } catch (ElasticsearchException) {
            $this->logger->error('Unable to index bulk batch.', [
                'bulk' => $bulk['body'],
            ]);
        }
        $bulk = [
            'body' => [],
        ];
        gc_collect_cycles();
    }
}

if (count($bulk['body']) > 0) {
    try {
        $this->elasticsearch->bulk($bulk);
    } catch (ElasticsearchException) {
        $this->logger->error('Unable to index bulk batch.', [
            'bulk' => $bulk['body'],
        ]);
    }
}
```

### QUERY

MUST -> AND
SHOULD -> OR
RANGE -> gt, gte, lt, lte

