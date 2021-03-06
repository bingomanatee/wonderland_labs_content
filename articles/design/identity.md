In databases, and especially in Ruby, the default identity field is `id: int AUTO INCREMENT`. It is so pervasive that figuring out (esp. in Ruby) how to deviate from this pattern is an amazing journey. 

I would suggest that in many cases this is a missed opportunity. Often in the source data there is a true canonical identity field that you can use as a primary key that not only satisfies the uniqueness clause but is more declarative. Case in point: I use an LDAP database as the basis for users at work. they have a 'login' field that is a string that serves wonderfully as an identity. If we'd started with using LDAP names as keys we'd have one fewer indexing chore, and if I looked at any record the user_id field would be, instead of an anonymous number, an informative string that would in some cases reduce the number of joins required to get meaningful data about the owner of a piece of data. 

Consider in this scenario that we script synchronization between the LDAP system and its SQL cache. If a user was delete and then restored, then we would have at one point a record `{id: 1001, ldap_name: 'jsmith'}` that we would have joined to related records. After the delete and recreation of his user record, we now have a record `{id: 1519, ldap_name: 'jsmith'}` -- so some records are joined to user_id 1519, and some records are joined to user_id 1001; but both in reality are related to jsmith. 

So every time you fail to use a real-world identity for the ID field you create potentials for diminished comprehension and multiplicity of relations to the same actual piece of information. 

Similarly, simple property tables such as list of states, zip codes, countries, languages etc. all have natural identities/real names that are unique and specific. Annotating them with a numeric identity just obscures the information. If you have a list of people, normalizing with a `country_id` just pointlessly encrypts the data where a straight value of 'country': 'US' is clear and unambiguous. 

Another classic example is time zones. They have unique identifiers already - the short time zone names. Even if you have a table of time zone records, there is no need to abstract them behind a database identifier when they come with predefined identity already.

Many real world transactionable items -- checks, invoices, publications, etc., all have native identifying digits or composite ids (your check ID is the check number in combination with the account ID). 

Using true identity values can even make your queries run faster by reducing the number of tables/relations in them.

## no REST for the wicked

Consider the case of REST or web services in general. Say you want to get comments for a user from an api that answers to `GET /api/users/[user_id]/comments`. You know in this case you want to get 'jsmith' comments. First you have to get the ID of 'jsmith' by calling `GET /api.users?login=jsmith` or some such to get the identity for a user that in fact you ALREADY HAVE. then you have to use that ID in the comments call above. 

## the hash identity revolution

Even if your data doesn't have a natural identity, integral id values are not necessarily the best option. I prefer hashes, AKA (UUIDs)[https://en.wikipedia.org/wiki/Universally_unique_identifier]. 

While [Mongo didn't invent the idea of hash identity](https://docs.mongodb.com/manual/reference/method/ObjectId/) it did popularize it. With hash identity there is no sequence of numbers assigned to the identity of a recordset/resource; instead a quasi-random hash is created for each record. The date of creation is assigned to some bits further making duplicates unique. 

What are the advantages to this? If you have a database split into shards/clusters, you can create records in either without having to track the nextId integral value. You can even, for instance, generate a new identity on the client. And unlike numbers, hashes are nigh-on impossible to guess so you can't "fish" a REST service easily for data or infer something about the size of a particular data set based on a record. 

## existential identity

Sometimes a records data is an identity. Take for instance a join table which joins foos and bars. The record is `{foo_id: [identity], bar_id: [identity]}`. Assuming that every combination is unique, what is the point of a separate identity key for the join? If you have a uniqueness index on those IDs and you want to break the association you'll be searching for all records with those values and deleting them. 

Similarly, if you have a dump table of log events with date, type and message you probably won't ever plan on deleting individual records; the usual use case is write and then search by date range always. the composite of message type, date and message are collectively enough identity. You probably will never graph out from a log table so why bother? 

## Leaf Records vs. JSON data

Its sometimes worthwhile questioning whether related tables are truly needed, as well. Sometimes related data is better off saved as a JSON cluster. PostGres provides this functionality. A classic example is a user record with phone numbers, e-mails, and addresses. In conventional SQL this can end up as a small village of linked tables. But embedding them as JSON fields in a single table is nearly always more practical. They remain searchable in postGres using JSON query operators and can radically reduce the joins needed to describe a record. 

JSON is also useful for freezing the state of related data at the time of a transaction. Denormalizing data in this fashion is useful for recording what the state of a given tree of data was when it was created. For instance, say a person purchases product 10009 which requires 3 AAA batteries, and has it sent to their home address. You can snapshot the state of product 10009 in the transaction, as well as the users' home. 3 days After that transaction (which was shipped), the user changes his home address in his user admin page. Also, the manufacturer changes the product, and it now requires 2 D batteries. (but not the product that was shipped.) 

The user doesn't get the product, of course and calls customer service. If the system was using purely relational data, the CSR might not have any clue why the product wasn't shipped to the new address, which is currently the value of the user home address record, after the user changed it. However since all that data is frozen in a JSON blob in the customer's transaction history, the CSR notes that the product was shipped to an address that is not the one in their current record, and they talk and determine that the customer's product was shipped to their old address. 

Also the customer asks what type of batteries it needs. Again, the CSR can look at the frozen data in the transaction and tells them the right sort of batteries, instead of being mislead by the difference between the currently shipping product and the one that was actually shipped.  

## The use of identity

Identity is useful in a finite set of scenarios: when

* The data has no real world unique identifier
* The net sum of the fields is non-unique -- as in the case of deposits, where multiple identical deposits might be made and need to be uniquely tracked
* You want to be able to delete specific records in the future
* You want to retain relational integrity while having the ability to change all the fields of a record except for its identity
* The identity of an item is secure and something you want to obscure from general perusal (like a social security number or a credit card number). 

But even in these scenarios, there are no upsides to using autoincrement numbers other than the fact that they are the default in many databases/ORMs. (And that is more of an indictment to those systems than anything else.)



