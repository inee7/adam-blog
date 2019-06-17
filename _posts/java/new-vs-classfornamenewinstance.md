The new operator creates a new object of a type that's known statically \(at compile-time\) and can call any constructor on the object you're trying to create. It's the preferred way of creating an object - it's fast and the JVM does lots of aggressive optimizations on it.

Class.forName\(\).newInstance\(\) is a dynamic construct that looks up a class with a specific name. It's slower than using new because the type of object can't be hardcoded into the bytecode, and because the JVM might have to do permissions checking to ensure that you have the authority to create an object. It's also partially unsafe because it always uses a zero-argument constructor, and if the object you're trying to create doesn't have a nullary constructor it throws an exception.

In short, use new if you know at compile-time what the type of the object is that you want to create. Use Class.forName\(\).newInstance\(\) if you don't know what type of object you'll be making.

