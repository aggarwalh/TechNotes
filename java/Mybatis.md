###DAO Layer Pitfalls
  - Forgetting to insert some member's data
  - Mapping typos between memberName and db col name.

###Debugging
  - Viewing dynamic sql that is formed from Mapper
     - Debug on PreparedStatement in doUpdate() and doQuery() on executor classes of `org.apache.ibatis.executor` package (Latest checked on mybatis-3.2.8)
     - Most common executor is SimpleExecutor.
