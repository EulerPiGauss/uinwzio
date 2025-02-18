<nav id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#orgheadline1">1. 简介</a></li>
<li><a href="#orgheadline2">2. 数据模型设计</a></li>
</ul>
</div>
</nav>


# 简介<a id="orgheadline1"></a>

flask-user 方便你处理关于用户的注册登录发送邮件密码找回等事宜。其github地址在 [这里](https://github.com/lingthio/flask-user/) 。

    pip install flask-user

# 数据模型设计<a id="orgheadline2"></a>

请参看文档的 [这里](https://pythonhosted.org/Flask-User/data_models.html) ，比如下面这个:

```python
# Define User DataModel
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)

    # User email information
    email = db.Column(db.String(255), nullable=False, unique=True)
    confirmed_at = db.Column(db.DateTime())

    # User information
    is_enabled = db.Column(db.Boolean(), nullable=False, default=False)
    first_name = db.Column(db.String(50), nullable=False, default='')
    last_name = db.Column(db.String(50), nullable=False, default='')

    def is_active(self):
      return self.is_enabled

# Define UserAuth DataModel. Make sure to add flask.ext.user UserMixin!!
class UserAuth(db.Model, UserMixin):
    id = db.Column(db.Integer, primary_key=True)
    user_id = db.Column(db.Integer(), db.ForeignKey('user.id', ondelete='CASCADE'))

    # User authentication information
    username = db.Column(db.String(50), nullable=False, unique=True)
    password = db.Column(db.String(255), nullable=False, default='')
    reset_password_token = db.Column(db.String(100), nullable=False, default='')

    # Relationships
    user = db.relationship('User', uselist=False, foreign_keys=user_id)

# Setup Flask-User
db_adapter = SQLAlchemyAdapter(db,  User, UserAuthClass=UserAuth)
user_manager = UserManager(db_adapter, app)
```

上面的代码将邮箱相关信息和认证信息分开，然后建立了one-to-one关系，一个用户只能有一个注册邮箱。然后注意UserAuth那里加入的 `UserMixin` 类。