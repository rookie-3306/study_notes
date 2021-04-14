#springboot的事务控制很简单,只要在想要想要控制事务的方法上加上@Transactional注解
#当然事务控制一般用于数据库的增删改.
	---------------------------------
	@Transactional
    @Override
    public int saveStudent(Student student) {
        return studentMapper.insertSelective(student);
    }
	---------------------------------