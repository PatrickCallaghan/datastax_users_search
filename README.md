This sample application is the show the use of synonyms and phoentic matching in DSE Seach.

To create the schema and user data please execute the following commands with a DSE Search enabled cluster running.

```
cqlsh -f cql/datastax_schema.cql 

cqlsh -e "copy datastax_demo.users from 'cql/users.csv'"

dsetool write_resource datastax_demo.users name=synonyms.txt file=synonyms.txt

dsetool create_core datastax_demo.users schema=users_schema.xml solrconfig=solr_config.xml  reindex=true
```

 
Phonetic Matching in DSE Search is done by Beider-Morse Phonetic Matching (BMPM)  

	https://lucene.apache.org/solr/guide/6_6/phonetic-matching.html#PhoneticMatching-Beider-MorsePhoneticMatching_BMPM_
	
```
cqlsh -e "select * from datastax_demo.users where solr_query = 'first_name:Debhora'";

cqlsh -e "select * from datastax_demo.users where solr_query = 'first_name:stevie'";
```
Ensure that mulitple users are returned and the first_name is different matches. 

When using addresses its useful to set up synonyms to allow users to search using 'ave' instead of 'avenue'.

Setting up a simple synonyms file with the following sample can easily help acheive this
```
NY,New York
St,Street,road
Ave,Avenue
Cres,Crescent
```

Ensure the following both return the same results
```
cqlsh -e "select * from datastax_demo.users where solr_query = 'city_name:\"New York\"'";

cqlsh -e "select * from datastax_demo.users where solr_query = 'city_name:\"NY\"'";
```

Similarly, we can search for avenue and ave, street and st, and ensure we get the same results.
```
cqlsh -e "select * from datastax_demo.users where solr_query = 'street_address:avenue'";

cqlsh -e "select * from datastax_demo.users where solr_query = 'street_address:ave'";
```
 
For more resources on DSE Search please go to 

	https://docs.datastax.com/en/dse/6.0/dse-admin/datastax_enterprise/search/searchTOC.html


