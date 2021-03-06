# gioc

[![License](https://img.shields.io/:license-apache-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Go Report Card](https://goreportcard.com/badge/github.com/vlorc/gioc)](https://goreportcard.com/report/github.com/vlorc/gioc)
[![GoDoc](https://godoc.org/github.com/vlorc/gioc?status.svg)](https://godoc.org/github.com/vlorc/gioc)
[![Build Status](https://travis-ci.org/vlorc/gioc.svg?branch=dev)](https://travis-ci.org/vlorc/gioc?branch=dev)
[![Coverage Status](https://coveralls.io/repos/github/vlorc/gioc/badge.svg?branch=dev)](https://coveralls.io/github/vlorc/gioc?branch=dev)

gioc is a lightweight Ioc framework,it provides register and factory and depend solution

## Examples

###  base function
```golang
import (
	"fmt"
	"github.com/vlorc/gioc"
)

func main() {
	container := gioc.NewRootContainer()

	//regiter int instance
	container.AsRegister().RegisterInstance(17, "age")

	// get a int type of 'age'
	fmt.Println(container.Resolve((*int)(nil), "age"))
}
```

###  base factory
```golang
import (
	"fmt"
	"github.com/vlorc/gioc"
	"github.com/vlorc/gioc/factory"
	"github.com/vlorc/gioc/types"
)

func main() {
	container := gioc.NewRootContainer()
	age := 17

	//regiter int of value factory,the same as RegisterInstance
	container.AsRegister().RegisterFactory(
	    factory.NewValueFactory(age),(*int)(nil),"age")

	//regiter int of func factory
	inc := factory.NewFuncFactory(func(types.Provider) (interface{}, error) {
		age++
		return age, nil
	})

	container.AsRegister().RegisterFactory(inc,&age,"inc")

	// singleton mode,convert the singleton factory
	container.AsRegister().RegisterFactory(
	    factory.NewSingleFactory(inc),&age,"once")

	// get a int type of 'age'
	fmt.Println(container.Resolve((*int)(nil), "age"))
	
	// get a age + 1
	fmt.Println(container.Resolve((*int)(nil), "inc"))
	
	// get a age,once + 1
	fmt.Println(container.Resolve((*int)(nil), "once"))
	fmt.Println(container.Resolve((*int)(nil), "once"))
}

```

## License

This project is under the apache License. See the LICENSE file for the full license text.

## Interface

+ Provider
	+ provides Factory discovery
+ Factory
	+ responsible for generating Instance
	+ The basic plant has a value factory, method factory, agent factory, single factory, type factory
+ Mapper
	+ Get the Factory by id
+ Binder
	+ the Factory is bound by id
	+ can be converted to ReadOnly Mapper
+ Register
	+ as a connection to Factory and Binder
	+ provides the registration method, which eventually matches the Type to the Factory
	+ register custom Binder, Mapper, Factory
+ Dependency
	+ for target type dependency analysis, collection integration
	+ is converted to an Injector by an instance
+ Injector
	+ and obtain the Instance padding based on the Dependency retrieval Provider
+ Builder
	+ is also a Factory
	+ Use the Factory to get the instance and Injector to solve the dependency
+ Container
	+ provides Register and Provider, and the parent container makes up traversal
	+ can be converted to a Provider for ReadOnly
	+ can be converted to Seal Container

# Key

**dependency, dependency injection, inversion of control**

# Reference
