Angular as publicized promises to manage large datasets with magic. However its magic doesn't scale; here is how I made it scale despite itself. 

Say you have a large number of financial transactions. Like, your entire banking history. And you want to sort, filter and paginate it. 

The angular approach would be a series of filters on a dataset embedded into the scope. This is a very bad and non-scalable approach as you will be running many functions over many records and this is going to inevitably take a very long time to compute -- long enough for the client to notice it. Long enough, for instance, for them to change some of the variables on which these functions depend, such as re-sorting while you are computing, compounding the problem. 

Here is my solution for this circumstance. 

## Do not store all your data in $scope.

Store your basic data in a Domain Model -- basically a class devoted to this problem. Ideally, put this class in a worker where it can compute without blocking DOM interactions. 

Every time a variable that drives the data production changes, emit a message to the Model.

1. When the search string changes a field change message is emitted to the Model. 
2. The model is in the rest state; it begins collecting the matching records. it is now in searchFilter state. 
3. When the sort column changes, a field change is emitted. As the calculation that is required after a sort column change is a downstream action from the filter process, the field change is notes but computation from step 2 continues. 
4. When the records are filtered they are passed to the next process, sort; however since there are fewer records, sort is now operating on the memoized data from the filter process, a much smaller set. We are now in Sort state. 
5. The results of the sort are memoized and passed to pagination; we are in pagination state.
6. The paginated sub-chunk of records are returned out of the Model and stored in the $scope as a virtual row. 

## Waterfall computation. 

All this is accomplished in a series of "waterfall" computing classes that accept memoized data from the previous waterfall process and pass the post-computed results to the next process in the chain. The classes do their work in bursts, so that if the upstream process are forced to recompute, all downstream computation is cancelled and waits for a memo input from an upstream process. This can be done by setting a maximum number of records to process before checking in with the state machine. 

Each of the waterfall computers are event emitters and state machines. They have the state idle, working, and done, which returns back to idle quickly. They emit messages on computingStart, onComputingDone to all downstream processes; the latter includes a memoized snapshot of the previous recordset. This is kept, so that, for instance, if a downstream process like pagination is fed a different set of parameters (go next page) the upstream computers don't have to uselessly re-process the data. 

The process are generally best put in ascending order of efficiency: filter(text search), filter(by category - indexed), sort (by field), paginate. This means that in general the faster processes run more often than the slower ones.

the end result is that your first computation cycle will be as long as it would using angular, but subsequent cycles are faster; some like repagination are extremely fast.