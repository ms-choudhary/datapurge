# datapurge
Tool which helps in deletion of data in Java EE environment

It is a full fledged java ee application for purging/deleting data in any enterprise application. 
Typical usage of deletion of data includes:

1. Certain data is rendered useless after definite period of time.
2. Can also be used for data clean up in testing environment.

It provides flexibility to delete the data rather than traditional way where user has to maintain delete queries of all tables to be deleted.
It handles deletion of related data as well, even if underlying database has not defined any foreign relation constraint.

This tool is also capable of extension in any way user sees fit. This document explores various design & usage aspects & how to extend it.

datapurge
|
|---- entities
|---- frontend
|---- deletion engine
|---- extension


Frontend:

This module is responsible for communication to the tool about the entries to be deleted. There are two ways to interact with the tool:-

1. DB 

This tool maintains a table, DATA_PURGE_INPUT, whose definition is like:

DATA_PURGE_INPUT:-
    short_name
    query
    timer

short_name:
It is a short concise human readable name useful for refering to the query in the logs & during extension.

query:
This is a JPQL query responsible for identification of data entries to be purged.

timer:
This denotes the interval in seconds in which query should be run. This is useful if there is any business needs which require that certain
data be deleted after definite interval of time.

Valid timer values are any positive number greater than 0.
If value is 0 or negative query is executed only once.

At the timeout of every timer, deletion engine fetches the entries in this table. So changing the entries dynamically affects it's execution
without server restart.

2. Webservice


Deletion engine:

This is responsible for integrating all the modules. 
Typical program is like:
