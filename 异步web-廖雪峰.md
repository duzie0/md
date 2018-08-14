# 异步web-廖雪峰

## ORM:

```python
class Field(object):
    def __init__(self,name,colnum_type):
        self.name = name
        self.colnum_type = colnum_type

class IntegerField(Field):
    def __init__(self,name):
        super(IntegerField, self).__init__(name,'bigint')

class StringField(Field):
    def __init__(self,name):
        super(StringField,self).__init__(name,'varchar(100)')
#元类
class ModelMetaclass(type):
    def __new__(cls, name,bases,attrs):
        if name == 'Model':
            return type.__new__(cls,name,bases,attrs)
        print('Found model:%s'%name)
        #保存映射关系
        mapping = dict()
        for k,v in attrs.items():
            if isinstance(v,Field):
                print('Found mapping:%s==>%s'%(k,v))
                mapping[k] = v
        for k in mapping.keys():
            attrs.pop(k)
        attrs['__mappings__'] = mapping
        attrs['__table__'] = name
        # Hello = type('Hello',(object,),dict(hello=hello))
        return type.__new__(cls,name,bases,attrs)

#基类，继承自dict,拥有其全部功能
#写法灵活 user['id']或user.id
class Model(dict,metaclass=ModelMetaclass):
    def __init__(self,**kwargs):
        super(Model, self).__init__(**kwargs)
    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError("'Model' object has no attribute %s"%key)
    def __setattr__(self, key, value):
        self[key] = value
    def save(self):
        fields = []
        params = []
        args = []
        for k,v in self.__mappings__.items():
            fields.append(v.name)
            params.append('?')
            args.append(getattr(self,k,None))
        sql = 'insert into %s (%s) values (%s)'\
              %(self.__table__,' '.join(fields),' '.join(params))
        print('SQL:',sql)
        print('ARGS:',str(args))

class User(Model):
    id = IntegerField('id')
    name = StringField('name')

u = User(id=123,name='LiSi')
u.save()
--------------------------------------------------------------------------------------------
输出：
Found model:User
Found mapping:id==><__main__.IntegerField object at 0x00000166C5C0CB00>
Found mapping:name==><__main__.StringField object at 0x00000166C5C0CB38>
SQL: insert into User (id name) values (? ?)
ARGS: [123, 'LiSi']
```

