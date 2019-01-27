# beam-quickstart-py

## Set up your environment

```bash
python --version
pip --version
pip install --upgrade virtualenv
```
See the [Virtualenv](https://virtualenv.pypa.io/en/stable/) for more details.

## Create and activate a virtual environment

```bash
pip install --upgrade virtualenv
virtualenv venv
. venv/bin/activate
```

## Download and install

```bash
pip install apache-beam[gcp,test,docs]
```

## Execute a pipeline

```bash
python -m apache_beam.examples.wordcount \
    --input gs://apache-beam-samples/shakespeare/* \
    --output counts
```

---

# Apache Beam WordCount Examples

> The WordCount examples demonstrate how to set up a processing pipeline that can read text, tokenize the text lines into individual words, and perform a frequency count on each of those words. The Beam SDKs contain a series of these four successively more detailed WordCount examples that build on each other. The input text for all the examples is a set of Shakespeare’s texts.
  
>  Each WordCount example introduces different concepts in the Beam programming model. Begin by understanding MinimalWordCount, the simplest of the examples. Once you feel comfortable with the basic principles in building a pipeline, continue on to learn more concepts in the other examples.
  
* `MinimalWordCount` demonstrates the basic principles involved in building a pipeline.
* `WordCount` introduces some of the more common best practices in creating re-usable and maintainable pipelines.
* `DebuggingWordCount` introduces logging and debugging practices.
* `WindowedWordCount` demonstrates how you can use Beam’s programming model to handle both bounded and unbounded datasets.

## MinimalWordCount example

> MinimalWordCount demonstrates a simple pipeline that uses the Direct Runner to read from a text file, apply transforms to tokenize and count the words, and write the data to an output text file.

Link: https://beam.apache.org/get-started/wordcount-example/#minimalwordcount-example

```bash
python -m apache_beam.examples.wordcount_minimal \
    --input gs://apache-beam-samples/shakespeare/* \
    --output counts
```
Code: https://github.com/apache/beam/blob/master/sdks/python/apache_beam/examples/wordcount_minimal.py

## WordCount example

>This WordCount example introduces a few recommended programming practices that can make your pipeline easier to read, write, and maintain. While not explicitly required, they can make your pipeline’s execution more flexible, aid in testing your pipeline, and help make your pipeline’s code reusable.

Link: https://beam.apache.org/get-started/wordcount-example/#wordcount-example

```bash

# DirectRunner
python -m apache_beam.examples.wordcount \
    --input gs://dataflow-samples/shakespeare/kinglear.txt \
    --output counts

# As part of the initial setup, install Google Cloud Platform specific extra components.
# pip install apache-beam[gcp]

export GCP_PROJECT=$(gcloud config get-value core/project)
export GCE_ZONE=$(gcloud config get-value compute/zone)

export ENVIRONMENT=russo-dataflow-dev
export ENVIRONMENT_NAME=$GCP_PROJECT'-'$ENVIRONMENT
export GCS_BUCKET=$GCP_PROJECT'-'$ENVIRONMENT_NAME'-bucket'



python -m apache_beam.examples.wordcount --input gs://dataflow-samples/shakespeare/kinglear.txt \
                                         --output gs://YOUR_GCS_BUCKET/counts \
                                         --runner DataflowRunner \
                                         --project YOUR_GCP_PROJECT \
                                         --temp_location gs://YOUR_GCS_BUCKET/tmp/


```

Code: https://github.com/apache/beam/blob/master/sdks/python/apache_beam/examples/wordcount.py

## DebuggingWordCount Example

> The DebuggingWordCount example demonstrates some best practices for instrumenting your pipeline code.

Link: https://beam.apache.org/get-started/wordcount-example/#debuggingwordcount-example

```bash
python -m apache_beam.examples.wordcount_debugging --input YOUR_INPUT_FILE --output counts

# As part of the initial setup, install Google Cloud Platform specific extra components.
pip install apache-beam[gcp]
python -m apache_beam.examples.wordcount_debugging --input gs://dataflow-samples/shakespeare/kinglear.txt \
                                         --output gs://YOUR_GCS_BUCKET/counts \
                                         --runner DataflowRunner \
                                         --project YOUR_GCP_PROJECT \
                                         --temp_location gs://YOUR_GCS_BUCKET/tmp/
```

## WindowedWordCount example

> The WindowedWordCount example counts words in text just as the previous examples did, but introduces several advanced concepts.

Link: https://beam.apache.org/get-started/wordcount-example/#windowedwordcount-example

```bash
python -m apache_beam.examples.windowed_wordcount --input YOUR_INPUT_FILE --output_table PROJECT:DATASET.TABLE

# As part of the initial setup, install Google Cloud Platform specific extra components.
pip install apache-beam[gcp]
python -m apache_beam.examples.windowed_wordcount --input YOUR_INPUT_FILE \
                                         --output_table PROJECT:DATASET.TABLE \
                                         --runner DataflowRunner \
                                         --project YOUR_GCP_PROJECT \
                                         --temp_location gs://YOUR_GCS_BUCKET/tmp/
```

## StreamingWordCount example

> The StreamingWordCount example is a streaming pipeline that reads Pub/Sub messages from a Pub/Sub subscription or topic, and performs a frequency count on the words in each message. Similar to WindowedWordCount, this example applies fixed-time windowing, wherein each window represents a fixed time interval. The fixed window size for this example is 15 seconds. The pipeline outputs the frequency count of the words seen in each 15 second window.

Link: https://beam.apache.org/get-started/wordcount-example/#streamingwordcount-example

```bash
python -m apache_beam.examples.streaming_wordcount \
  --input_topic "projects/YOUR_PUBSUB_PROJECT_NAME/topics/YOUR_INPUT_TOPIC" \
  --output_topic "projects/YOUR_PUBSUB_PROJECT_NAME/topics/YOUR_OUTPUT_TOPIC" \
  --streaming

# As part of the initial setup, install Google Cloud Platform specific extra components.
pip install apache-beam[gcp]
python -m apache_beam.examples.streaming_wordcount \
  --runner DataflowRunner \
  --project YOUR_GCP_PROJECT \
  --temp_location gs://YOUR_GCS_BUCKET/tmp/ \
  --input_topic "projects/YOUR_PUBSUB_PROJECT_NAME/topics/YOUR_INPUT_TOPIC" \
  --output_topic "projects/YOUR_PUBSUB_PROJECT_NAME/topics/YOUR_OUTPUT_TOPIC" \
  --streaming
```


## Resources

[Apache Beam Python SDK Quickstart](https://beam.apache.org/get-started/quickstart-py/)

[Apache Beam WordCount Examples](https://beam.apache.org/get-started/wordcount-example/)

[Apache Beam Learning Resources](https://beam.apache.org/documentation/resources/learning-resources/)

[Python Development Environments for Apache Beam on Google Cloud Platform](https://medium.com/google-cloud/python-development-environments-for-apache-beam-on-google-cloud-platform-b6f276b344df)
