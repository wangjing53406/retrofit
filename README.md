Retrofit
========

Reusable Java and Android code from Square, Inc.

Modules:

 *  IO - Utility classes for doing low-level java I/O.
 *  HTTP - Abstracts away the messy logic of making network calls (depends on IO).


Http Usage
----------

Create an interface for your API.  You can create as many of these interfaces as you like.  For
each interface you create, calling `RestAdapter.service(MyInterface.class)` will create an
instance of that API handler, which you can then store and use throughout your application.  An
example interface:

    public interface DummyService {
      // Produces a url like "foo/bar?id=idValue".
      @GET("foo/bar")
      void normalGet(@Named("id") String id, Callback<SimpleResponse> callback);

      // Produces a url like "foo/idValue/bar?category=categoryValue".
      @GET("foo/{id}/bar")
      void getWithPathParam(@Named("id") String id, @Named("category") String category, Callback<SimpleResponse> callback);

      // Produces a url like "foo/bar/idValue" and body like "id=idValue&body=bodyValue".
      @POST("foo/bar/{id}")
      void normalPost(@Named("id") String id, @Named("body") String body, Callback<SimpleResponse> callback);

      // Produces a url like "foo/bar/idValue" and body generated by MyJsonObj.
      @POST("foo/bar/{id}")
      void singleEntityPost(@SingleEntity MyJsonObj card, @Named("id") String id, Callback<SimpleResponse> callback);
    }

Note that each method _must_ have a `Callback` object at the end of the parameter list.  This is how
your application will handle the results of your network calls: errors and successful responses are
both handled by the `Callback` interface.

If you want to use the `@SingleEntity` method of specifying request body (see `singleEntityPost` above),
your `MyJsonObject` will need to implement `TypedBytes`.  For convenience, you can extend
`GsonRequestEntity` if you're just trying to send a JSON string in the request body.

Also worth noting: for POST/PUT requests using default form encoding for the request entity (see
normalPost), any path parameters are also included in the request body.  This is different from the
behavior of GET/DELETE, where path parameters are excluded from the query string.



Compilation
-----------

Retrofit is built using Maven and there is a very minimal amount of setup required for compilation.

Two environment variables are required which point to your Android SDK and native Android SDK. A common
place to put these is in a `.bash_profile` file in your home directory.

    export ANDROID_HOME=~/dev/android-sdk
    export ANDROID_NDK_HOME=~/dev/android-ndk

With these two environment variables loaded you can compile the modules and sample application by running
`mvn clean verify`. Each module's built artifact will be in its respective `target/` folder.
