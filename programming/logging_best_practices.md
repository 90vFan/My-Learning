Logging Best Practices
---------------------------

Logs are like evidence in crime scenes, and developers are like CSIs（犯罪现场调查）.

Same as how lack of evidence leads to cold cases(悬案), lack of meaningful logs makes troubleshooting time-consuming or even impossible.

## think a little more about logging

1. "What should I log?".
   
    Logging is the process of recording application **actions and state** to a secondary interface.

2. importance of context
   
    add username/account

3. how to deal with multiple operating concurrently?
   
    add session-id/request id

4. Why not log everything?
   
    With the right level of context information available, it is an easy task to re-trace the program execution - fortunately, computer programs are deterministic!
    
    avoid the log became a chaos of information

5. Not all exceptions are errors
   
    some exceptions are anticipated or even expected, not necessary to log it in that high level


## Best Practices

1. Log at the Proper Level

   - error
   - warning
   - info
   - debug

2. Employ the Proper Log **Category**

   包括正确的分类

   Example: session-id

   ```sh
   session-id:EFS93-SEFRD-34WES-343DD main:gateway:132 - request is xyz
   ```

3. Log **critical state transition**

   记录关键信息，比如状态转换等

   Logs should contain sufficient information to help with the reconstruction of **state transitions**.

   Bad example:

   ```sh
   [2020–04–20T03:36:57+00:00] server.go: Error processing request
   [2020–04–20T03:36:57+00:00] server.go: SSN rejected
   [2020–04–20T03:36:57+00:00] server.go: SSN rejected for user UUID “123e4567-e89b-12d3-a456–426655440000”
   ```

   Good Example:

   ```sh
   [2020–04–20T03:36:57+00:00] server.go: Received a SSN request(track id: “e4a49a27–1063–4ab3–9075-cf5faec22a16”)
   from user uuid “123e4567-e89b-12d3-a456–426655440000”(previous state), rejecting it(next state) because the 
   server is expecting SSN format AAA-GG-SSSS but got **-***(why)
   ```

4. Write **Meaningful** Log Messages

   记录有用的信息，而且可以在故障检查的过程中提供帮助

   - Don’t Log Too Much or Too Little
   - log message 不要依赖于前一条 log，因为前一条 log 可能不会显示或显示在不同的地方（category, level, multi-threaded)
   - You should know **what is going on** with the log when you diagnose an issue

   Bad example:

   ```sh
   status: completed
   ```

   Good example:

   ```sh
   Transaction 1232034 deployment status: completed
   ```

5. Add **Context** to Your Log Messages

   **脱离代码上下文环境后依然可以被读懂**

   Bad example:

   ```
   Transaction failed
   
   xyz file is not found
   
   IndexOutOfBoundsException
   ```

   *Logging cannot be isolated with exception handling*

   Good example:

   ```
   Transaction 2346432 failed: cc number checksum incorrect
   
   could not load configuration details for DB: file ‘xyz’ is missing
   
   IndexOutOfBoundsException: index 12 is greater than collection size
   ```

   *Audience should know what is going on with the log when you diagnose an issue*

6. Log in **Machine Parseable** Format

   log中的数据机器可解析

   Bad example:

   ```sh
   log.info("User {} plays {} in game {}", userId, card, gameId);
   ```

   Good example:

   ```sh
   2013-01-12 17:49:37,656 [T1] INFO  c.d.g.UserRequest  User plays 
   {'user':1334563, 'card':'4 of spade', 'game':23425656}
   ```

   *For example, could be parsed for Graylog or ElasticSearch*

7. But Make the Logs **Human-Readable** as Well

   易读易懂

   Who wanted to read through could understand what is going around.

8. Think of Your **Audience**, includes: end-user, system-administration, developer

考虑**听众**，换位思考

9. Don’t Log for Troubleshooting Purposes Only

   日志不只服务于故障定位

   - Auditing 审计，展示重要事件，业务需求（business requirement)
   - Profiling 信息剖析
     * 一个操作、过程的开始和结束
     * performance metric (响应时间，访问频率，用户计数，程序性能指标, etc)
   - Statistics 统计
     * 特定事件
     * 用户行为
     * 程序性能
     * 错误监测用以报警(for Graylog, ElasticSearch)
     * etc


> *“Everyone knows that debugging is twice as hard as writing a program in the first place. So if you’re as clever as you can be when you write it, how will you ever debug it?” — Brian Kernighan*
