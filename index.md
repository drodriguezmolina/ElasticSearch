## Welcome to ElasticSearch Guide

Welcome to the definitive guide of ElasticSearch.
In this page I show the most important things of ElasticSearch.

### Index

The first step to use ElasticSearch is index the documents.

Bulk instance

```markdown
$bulk = [ 'body' => [], ];

foreach ($results as $key => $result) {
    $result['RESULTID'] = $result['BASTIDOR'] . $result['TESTSTART'] . $line . $result['BT_VARIANT'];
    $result['LINE'] = $line;

    $bulk['body'][] = [
        'index' => [
            '_index' => $indexName,
            '_id' => $key . $line,
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
