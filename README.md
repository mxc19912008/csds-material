# Data Lab

*Assigned: *

*Due: , 2016, 12:59 PM (just before class)*

The goal of this lab is for you to experiment with working with data
in various degrees of structure.

You will work with a dataset of Tweets encoded in multiple ways to compute
some summary information and reflect on the pros and cons of each
approach.

### Setup

To setup the tools required for this lab, you'll be running a preconfigured VM that has been made especially for this lab.
Start by installing [Vagrant](https://www.vagrantup.com/downloads.html) and [Virtualbox](https://www.virtualbox.org/wiki/Downloads). If you've done the Hadoop homeworks, you should already have Virtualbox installed.

Once Vagrant is installed, check your installation by running `vagrant -v` on the command line. 
```
$ vagrant -v
Vagrant 1.7.4
```
The next step is to clone this repository on your computer.
```
$ git clone -b ewu-csds https://github.com/prakhar1989/csds-material 
```
Now that we have everything, we can start the VM.

```
$ cd csds-material
$ vagrant up
```
You will see a stream of text run by on you on your screen. Vagrant will download the VM and install the required packages. If everything works as expected, you should now be able to `ssh` into your VM using `vagrant ssh`. Remember, always `cd` into the directory containing the `Vagrantfile` for the vagrant commands to work.

Once you are done with using the VM, disconnect from the VM, and then run `vagrant halt` to turn it off.

### Step 1: Look at some JSON-encoded Tweets

First decompress the Tweets data file:

    gzip -d twitter.json.gz

`twitter.json` contains a JSON-encoded tweet on each line.  Check out
[twitter's documentation](https://dev.twitter.com/docs/platform-objects/tweets)
for information about the tweets or simply read the file.  Take a look
at this file.  *You won't be using it directly, but we've written some
scripts to process it for use in future steps.*

By reading `twitter.json`, you've completed Step 1!

In the following steps, you will use three systems to perform data
analysis: Protocol Buffers, a relational database called SQLite, and
MongoDB.  Using each of the three systems, you're going to answer the
following four questions:

1. Find the number of deleted messages in the dataset.
1. Find the number of tweets that are replies to another tweet.
1. Find the five user IDs (field name: `uid`) that have tweeted the most.
1. Find the names of the top five places by number of tweets.  (Tweets may have a "place" attribute that describes where the tweet is from).

### Step 2: Analyses using Protocol Buffers

In this step, you will use protocol buffers to perform the analyses.

For details about protocol buffers, [read this
page](https://developers.google.com/protocol-buffers/docs/overview).
In essence, it's helpful within a company to know the schema of data
and messages being stored and transmitted by various services.
Protocol buffers, Thrift, and Avro are all projects that help define
schemas and compact/efficient serialization formats for your data.
None of these tools help you process the data with query languages and
data processing frameworks: those are built on top of these libraries.

The protocol buffer definitions are found in `twitter.proto` and the
data files are in `twitter.pb`.

We generated the protocol buffer's python bindings by running:

    protoc twitter.proto --python_out=.
    
This should generate a `twitter_pb2.py` that you can `import` in your
python scripts.

`twitter.pb` is generated by calling `python encode.py`, but you do
not need to re-create the file.  Note that this script does not
include all fields: we serialized only the subset of fields that are
necessary for answering the questions.

*Perform the four analyses from Step 1 using protocol buffers.  Keep a
copy of your code and the answers. You will have to write some code to
process the protocol buffer-encoded data.  There are [official
C++/Java/Python
libraries](https://developers.google.com/protocol-buffers/docs/reference/overview)
you can use, as well as [other language implementations in the
third-party
listings](https://code.google.com/p/protobuf/wiki/ThirdPartyAddOns).*

### Step 3: Analyses on database records

In this step, you will be working with the twitter data encoded as a
sqlite3 database file.  The file, `twitter.db` was generated using
`python createdb.py`, but you do not need to re-run this script.  The
schemas are defined in `twitter.ddl`. Note that this schema does not
include all fields: we includes only the subset of fields that are
necessary for answering the questions.

Start a sqlite3 prompt by typing:

    sqlite3 twitter.db

For SQL help, refer to the fantastic [sqlite documentation](http://www.sqlite.org/docs.html)
and [postgresql's documentation](http://www.postgresql.org/docs/).

*Perform the four analyses listed in Step 1 using sqlite.  Keep a copy
 of your code and the answers.*

### Step 4: Analyses in MongoDB

In this step, we will import and query the JSON data we've collected
in MongoDB.  First, let's get the data into Mongo:

    mongoimport -d lab2 -c tweets twitter.json

This will, in the database `lab2`, create a collection called `tweets`
from the JSON blobs inside `twitter.json`.

To access the `lab2` database, type

    mongo lab2

Refer to Mongo's detailed [query language documentation](http://docs.mongodb.org/manual/reference/method/db.collection.find/#db.collection.find) for help.

*Perform the four analyses listed in Step 1 using MongoDB.  Keep a
 copy of your code and the answers.*

Note: 

1. You can use any programming language of your choice to interact with MongoDB or simply run queries from within the Mongo CLI. 
2. If you are not using the Mongo CLI, then you should not call the entire collection, like doing docs = "SELECT * FROM table", and then perform filtering on docs to get the solution. Marks will not be given for that. 
3. It is required of you to query the database that extracts the complete solution or atleast a partial solution followed by some processing to eventually get the complete solution.

### Step 5: Reflection

1. Read the schema and protocol buffer definition files.  What are the main differences between the two?  Are there any similarities?
1. Describe one question that would be easier to answer with protocol buffers than via a SQL query.
1. Describe one question that would be easier to answer with MongoDB than via a SQL query.
1. Describe one question that would be easier to answer via a SQL query than using MongoDB.
1. What fields in the original JSON structure would be difficult to convert to relational database schemas?
1. In terms of lines of code, when did various approaches shine?  Think about the challenges of defining schemas, loading and storing the data, and running queries.
1. What other metrics (e.g., time to implement, code redundancy, etc.) can we use to compare these different approaches?  Which system is better by those measures?
1. How long did this lab take you?  We want to make sure to target future labs to not take too much of your time.




### Handing in your work

You should create a text file with your name, the results of the four
analyses from Step 1 as run on the three systems in Steps 2, 3, and 4,
and brief responses to the reflection questions in Step 5.  Where
possible, show the query you used (describe it at a high level if the
code/query takes up more than a few lines).  Upload your writeup to
the [course Stellar
site](http://stellar.mit.edu/S/course/6/fa13/6.885/) as the "lab2"
assignment.



### Feedback (optional, but valuable)

If you have any comments about this lab, or any thoughts about the
class so far, we would greatly appreciate them.  Your comments will
be strictly used to improve the rest of the labs and classes and have
no impact on your grade. 

Some questions that would be helpful:

* Is the lab too difficult or too easy?  
* Did you look forward to any exercise that the lab did not cover?
* Which parts of the lab were interesting or valuable towards understanding the material?
* How is the pace of the course so far?
