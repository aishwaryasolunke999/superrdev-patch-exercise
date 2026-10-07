# Notes

## Summary of changes
1. **Search/status filter ignored (backend + Oracle):** AND/OR precedence in the query meant status and archived checks only applied to one half.
2. Added brackets in `TaskRepository.java` and in both queries in the Oracle file.
3. **Artificial delay:** removed a `Thread.sleep` in `TaskController` that slowed short searches.
4. **Invalid status caused a 500:** now returns 400 with a clear message.
5. **Unsafe paging:** `page` is clamped to at least 1 and `pageSize` to 1-100.
6. **Page not reset on filter change (frontend):** changing search or status now returns to page 1.
7. **Race condition in `useTasks`:** stale responses are ignored using a cancelled flag in the effect cleanup.
8. **Loading/error state:** loading now always turns off, and old errors are cleared on the next request.
9. **No debounce:** search requests now wait 300 ms after typing stops.

## What I chose not to change
- Escaping `%` and `_` in search. They act as wildcards, but the query is parameterised, so it is not an injection risk. I skipped it because of the time limit.
- Paging is still done in Java after loading all matching rows. Fine for about 50 tasks, so I left it.
- I could not run the Oracle file locally, so that fix was checked by reading only.

## Tools used
I used Claude to help locate the bugs and explain why they happen. 
I applied and tested each fix myself (browser, API URLs, Network tab) and wrote the handwritten explanations myself. 
