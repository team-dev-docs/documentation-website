# OpenSearch Documentation 2.19.0 Release Notes

## Replacing Thread::getId() with Thread::threadId()

In OpenSearch 2.19.0, we've made an important update to improve compatibility with newer Java versions and eliminate deprecation warnings. This change involves replacing deprecated usages of `Thread::getId()` with `Thread::threadId()`, which was introduced in Java 19.

### Key Changes

- The `Thread::getId()` method has been replaced with `Thread::threadId()` throughout the codebase.
- This change affects various components of OpenSearch, including common utilities, language expressions, monitoring, task management, and thread pool operations.
- The update is scoped to actual `Thread` usage and does not affect domain-specific `getId()` calls on objects like `Task`, `Node`, etc.

### Affected Components

The following components have been updated:

1. Common utilities
2. Language expressions
3. JVM monitoring
4. Query profiling
5. Task resource tracking
6. Thread pool management
7. Test cases

### Impact and Benefits

- **Improved Compatibility**: This change ensures that OpenSearch remains compatible with newer Java versions, particularly Java 19 and beyond.
- **Deprecation Warning Removal**: By adopting the new `threadId()` method, we've eliminated deprecation warnings related to `Thread::getId()`.
- **Future-Proofing**: This update helps in maintaining the codebase and prepares OpenSearch for future Java updates.

### Developer Notes

If you're working with OpenSearch code or developing plugins, please note the following:

- When dealing with thread IDs, use `Thread.currentThread().threadId()` instead of `Thread.currentThread().getId()`.
- This change does not affect the behavior of the code; it's a straightforward replacement of the method call.
- Existing tests have been updated to reflect this change, ensuring continued reliability of the codebase.

### Conclusion

This update is part of our ongoing efforts to keep OpenSearch up-to-date with the latest Java developments. While it doesn't introduce new features, it's an important maintenance update that helps ensure the long-term viability and performance of OpenSearch.

For more detailed information about this change, you can refer to the [pull request #18237](https://github.com/opensearch-project/OpenSearch/pull/18237) in the OpenSearch GitHub repository.