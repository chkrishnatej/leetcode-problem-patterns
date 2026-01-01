# Majority In Stream

The Classic Boyers-Moore Algorithm for n/2 can be used as is.

At any time, `candidate` is the **current leader**. If the stream **ends**, you must do a quick pass (or use a running verification) to check if it really is > n/2.

The beauty here is **O(1) memory usage,** you can handle gigabytes or endless data without storing it. But in a truly **infinite stream**, you can only keep the _most probable_ candidate so far there’s no “final majority” unless you define a fixed window size.
