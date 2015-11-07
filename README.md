# repr-ld

Repr-ld es una librería de nodejs que te permite serializar y deserializar objetos de javascript al formato JSON-LD de manera sencilla y sin nada de boilerplate. repr-ld es completamente framework-agnóstico y, por lo tanto, puede ser integrado a cualquier ORM o framework web. Recomendamos el uso de repr-ld para la creación y, especialmente, para el prototipado rápido de APIs REST. Invierte más tiempo desarrollando features que generen valor para tus clientes y menos tiempo escribiendo código boilerplate para tu aplicación!

## Installation

This should be a piece of cake, just run in your project folder:

``npm install repr-ld --save``

and there you are :)

## repr-ld 101

Esta es una implementación del patrón [Representer](http://nicksda.apotomo.de/2011/12/ruby-on-rest-introducing-the-representer-pattern/). Básicamente, digamos que hay un tipo de objeto (AKA el modelo) que te gustaría poder serializar a JSON-LD (at this point you might be wondering [why JSON-LD](#why-json-ld)) y que te gustaría poder deserializar (ie, construir) a partir de un documento de JSON-LD. El patron Representer te permite tener otro objeto (AKA el representer) que se encargue de la serializacion/deserialización. Siendo más concretos, supongamos que tenemos un modelo que sigue el formato 

```
person = {
  id: ...,  // the id, the only "magical" property here
  firstName: ...,
  lastName: ...,
  gender: ...,
  age: ...,
  job: ...
}
```

Entonces podemos definir el siguiente representer

```
reprLd = require('repr-ld');
personFactory = require('./person').factory;

myReprLd = reprLd();
personRepresenter = myReprLd.define({
  name: 'person',
  factory: personFactory,
  properties: {
    firstName: true,
    lastName: true,
    gender: true,
    age: true,
    job: true
  }
});
```

y luego podemos serializar/deserializar

```
// Serialization. Btw, model is a model instance.
// It returns a javascript object representing a JSON-LD
// document which can be trivially turned into a String using
// the JSON.stringify function.
personRepresenter.serialize(model)

// Deserialization. Btw, obj must be a javascript object
// which represents a JSON-LD document.
// It returns a model instance.
personRepresenter.deserialize(obj)
```

Pretty cool, huh?

But... what if you wanted to have a different ``id`` property, or maybe to have some read-only property, or at the same time to do some validation, and return only if all validations pass else throw an exception? Don't worry, Repr-ld is flexible enough to deal with all those issues and many more.
