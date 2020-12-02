# Week02 作业

## 我们在数据库操作的时候，比如 dao 层中当遇到一个 sql.ErrNoRows 的时候，是否应该 Wrap 这个 error，抛给上层。为什么，应该怎么做请写出代码？

dao层遇到一个 sql.ErrNoRows 的时候，应该 Wrap 这个 error 抛给上层，上层即 service 层根据具体的业务场景，来决定对这个 error 如何处理。

```go
// Dao
fun (userDao *UserDao) FindById(id int) (user *User, err error) {
  user, err = db.get(id)
  return user, errors.Wrap(err, "find user err")
}
// Service
fun (userService *UserService) FindById(id int) (user *User, err error) {
  user, err := userService.userDao.FindById(id)
  if err != nil {
    log.error(err)
    return nil, err
  }
  return user, err
}
```

