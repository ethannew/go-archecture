```go
// DAO 层
type Dao struct {}

func New() *Dao {
	return &Dao{}
}

func (d *Dao) FindById(id int) (user *model.User, err error) {
	err = DB.Table("user").Where("id = ?", id).Find(user).Error
	
	if err != nil {
		err = errors.Wrap(err, fmt.Sprintf("FindByIdErr:%v", id))
	}
	return
}
```

```go
// Service 层
type Service struct {}

func (s *Service) FindById(id int) (user *model.User, err error) {
	user, err = dao.FindById(id)
	if errors.Is(err, dao.ErrRecordNotFound) {
		user = dao.GetDefault()
		err = nil
	}
	return
}
```