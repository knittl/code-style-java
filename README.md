# Opinionated, modern code style for Java

## Goals

* Readable
* Clear
* Expressive
* Understandable
* Scanable
* Minimal diffs
* Easily edited

## Quick guidelines

* Max 100 - 120 chars per line
* Prefer `final` (except lambda args)
* Prefer method references over lambdas
* Operators on next line
* Separate (logical) blocks with empty lines
* Empty line after "jumps" (loops, `return`, `break`, `continue`)
* "Chop" long expressions, arguments on separate lines
* Continued lines indented twice
* Mark methods without state as `static`

# Example

```java
@Multiple(annotations = "")
@With(
    several = "property",
    values = {
        "one",
        "two",
        "three"
    })
public class MyClass extends MyBase implements Implementable {

    public static final String CONSTANT_VALUE = "compile-time-constant";

    private static final long CLASS_ID = 1337L;

    private final MyDependency dependency;

    public MyClass(final MyDependency dependency) {
        this.dependency = Objects.requireNonNull(dependency);

        final MyCache myCache = new MyExpiringCache(10, MINUTES);
        final MyOverlyLongName overlyLongInstance
                = new MyOverlyLongName(myCache, "arguments");
        final MyThingWithTooManyArgs = new MyThingWithTooManyArgs(
                myCache,
                overlyLongInstance,
                dependency,
                "and",
                "some",
                "more",
                "arguments");
    }

    public static void doSomethingWithManyParameters(
            final String action,
            final int iterations,
            @Annotated
            final Object payload) {
        if (payload == null) {
            return;
        }

        if (iterations < 0) {
            return;
        }

        for (int i = 0; i < iterations; ++i) {
            call(action, payload);
        }

        callAnotherMethodWithManyParameters(
                action,
                iterations,
                "foo",
                "bar",
                13,
                37);
    }

    public <T> void streams(final Collection<? extends T> elements) {
        final List<T> list = elements.stream()
                .filter(Objects::nonNull)
                .map(x -> transform("mapping", x))
                .collect(Collectors.toList());

        dependency.send(list);
    }

    @Override
    public boolean equals(final Object other) {
        return other instanceof final MyClass that
                && Objects.equals(that.dependency, dependency);
    }

    @Override
    public int hashCode() {
        int hash = Objects.hashCode(dependency);
        hash = 31 * hash + Objects.hashCode(/*another dependency*/);
        return result;
    }

    private static final class NestedClass
            extends AnotherBase
            implements Implementable,
                    Nestable {
        private NestedClass() {
        }

        public String getName() {
            return "name";
        }

        public int age() {
            return 42;
        }
    }
}
```

## Open discussions

* Star-imports vs explicit imports
* Naming of getters: Java beans `getValue()` vs record-style `value()`
* `final` with `instanceof` operator
* `final` with try-with-resources
