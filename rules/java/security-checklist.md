---
paths:
  - "**/*.java"
---
# Java 后端军规（个人清单）

> This file extends [common/security.md](../common/security.md) with Java-specific
> checklist items distilled from 蚂蚁军规 V2.0. Applies to Spring Boot / Java
> backend code.

## 1. 金额计算禁用浮点数

- 涉及金额计算，禁用 `float` / `double`，一律用 `BigDecimal` 或专用金额类型，避免精度丢失导致资损。

## 2. 数据库更新：一锁二判三更新

- 并发更新数据库记录时：先锁定记录（一锁）→ 基于锁定后的最新状态做业务判断（二判）→ 判断通过再更新（三更新）。
- 正向、逆向业务并发时锁顺序严格一致，避免死锁；一般锁定领域模型根单据。

## 3. 禁用无界线程池与无界队列

- 禁止 `Executors.newCachedThreadPool()` 和 `Executors.newFixedThreadPool()` 等无界线程池/队列。
- 线程池用**有界队列**，队列长度不宜过大。

## 4. ThreadLocal 用完必须 remove

- `ThreadLocal` 使用完毕后在 `finally` 块中调用 `remove()` 清理。
- `set(null)` 不算清理，仍会内存泄漏。

## 5. SQL 禁用 SELECT * 和字符串拼接参数

- 查询用明确列名，禁用 `SELECT *`。
- 参数化查询：MyBatis 用 `#{}`，禁用 `${}`；iBatis 用 `##`，禁用 `$$`，防 SQL 注入。

## 6. 反序列化输入需白名单校验

- 禁止反序列化不受信任的数据；反序列化前用白名单校验类，重写 `ObjectInputStream.resolveClass()`。

## 7. 接口与参数需做权限校验

- 接口需校验当前用户是否有权访问/操作目标资源，防水平越权（操作他人数据）和垂直越权（低权调高权接口）。
- 未接入登录认证的接口不应对外开放。

## 8. 异步方法必须捕获异常

- `@Async`、线程池 `submit`/`invokeAll`、`CompletableFuture.runAsync/supplyAsync`、`Thread.start` 等异步任务必须 try-catch 或注册全局异常处理器，不可静默吞异常。
- 无返回值异步方法：方法内 catch 或注册 `AsyncUncaughtExceptionHandler`；有返回值：上层 `future.get()` 捕获。

## 9. 禁止空 catch 块

- 捕获异常必须处理（记录日志、通知用户、补救等），禁止空 catch 块静默吞异常。

## 10. 父子类禁止同名属性

- 父子类禁止定义同名属性，避免框架注入只注入子类、父类方法读到父类旧值导致 NPE 或行为异常。