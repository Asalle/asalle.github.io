---
layout: post
title:  "Lack of generics in go and a way to go around it in one particular case"
date:   2019-09-29 20:37:20 +0100
categories: programming
tags: go, generics, clojure
---

Let's look into this particular case of handling two different things in our db:
we have apples and we have bananas and we want to check if they are in the db and if so -- 
remove them. My background is C++, where one could do something like this:

{% highlight c++ %}
class Apple{ 
    std::string id;
    public:
    static constexpr std::string_view table = "apple_tbl";
};
class Banana{ 
    std::string id;
    public:
    static constexpr std::string_view table = "banana_tbl"; 
};

class DBDriver 
{
  template<typename T>
  bool has() 
  {
    return db->query("Do you have %s?", T::table);
  }
  
  bool remove(std::string_view id) 
  {
    return db->query("Remove %s of type %s then!", id, T::table)
  }

  private:
  mysql::driver *db;
};
{% endhighlight %}

So what is there to do in go? I figured, if we have a function that takes an `apple-` or `bananaID` we can just partially apply it and pass it into a more general `remove` function like so and have minimal repeating code:

{% highlight go %}
package main

type driver struct {}
func (d *driver) hasApple(appleID string) bool {
    // some quering
    return false
}
   
func (d *driver) hasBanana(bananaID string) bool {
    // some quering
    return false
}
    
func (d *driver) deleteApple(appleID string) {
    // some deletion
}

func (d *driver) deleteBanana(bananaID string) {
    // some deletion
}

var db = driver{}

func deleteFruit(get func() bool, remove func()) {
    if get() {
        remove()
    }
}


func DeleteApple(appleID string) {
    get := func() bool {
        return db.hasApple(appleID)
    }
    remove := func() {
        db.deleteApple(appleID)
    }
    
    deleteFruit(get, remove)
}

func DeleteBananas(bananaID string) {
    get := func() bool {
        return db.hasBanana(bananaID)
    }
    remove := func() {
        db.deleteApple(bananaID)
    } 
    deleteFruit(get, remove)
}
{% endhighlight %}

Does it work? Yes! Is it a good practice/idea? You tell me! @JimboTheJam1

