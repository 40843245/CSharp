# CH3 -- specifies the action
## objectives
In this article, you will learn how to

+ specifies the action of mapping table

## CH3.1 -- specifies the action of mapping table
+ To specify the action of mapping from one member of one class to other member of other class,

Invoke `ForMember` instance method followed by `CreateMap` method. 

```
        cfg.CreateMap<User, UserDTO>()
                   .ForMember(userDTO => userDTO.USERNAME, action => action.MapFrom(user => user.username))
```

+ To ignore the action (i.e. one member of one class will NOT to other member of other class)

Invoke `Ignore` instance method in the lambda expression about action (which passed in 1th argument (zero-based)) inside `ForMember` instance method call.

```
            cfg.CreateMap<UserDTO, User>()
                   .ForMember(user => user.birthdate, action => action.Ignore())
```

## reference
### further reading
+ [`Projection`](https://docs.automapper.org/en/stable/Projection.html)
+ [`Reverse Mapping and Unflattening`](https://docs.automapper.org/en/stable/Reverse-Mapping-and-Unflattening.html)
