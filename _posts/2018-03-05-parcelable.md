---
title: "BottomAppBar 자바로 구현하기"
date: 2018-03-05
categories: android
---

# # Parcelable과 Serializable 비교
Android에서 **activity 간에 데이터를 전달**하기 위해서는 전달하는 쪽에서는 putExtra(), 받는쪽에서는 getExtra()를 사용합니다. 전달하고자하는 값이, 한 두개라면, 값을 각각 putExtra()에 넣어서 전달하면 됩니다. 그러나 **전달해야하는 값이 많아 진다면(e.g. Object 단위) 직렬화 작업**이 필요하게 됩니다. Parcelable 또는 Serializable을 사용 할 수 있는데요. **Parcelable 이 전달 시간에 있어, 더 짧기때문에 Parcelable을 사용**해야 합니다.

>원문 링크
<https://android.jlelse.eu/parcelable-vs-serializable-6a2556d51538>
___
# Usage
>### Parcelable
[Parcelable](https://developer.android.com/reference/android/os/Parcelable.html)은 자바 표준 interface가 아니고, **Android SDK**에 포함 되어있습니다.
아래가 예제 클래스이며, reflection을 사용하지 않기위해서, Override 메서드가 포함되었고,Parcelable.Creator 을 생성해주어야 합니다. Serializable에 비해 빠른 성능을 내기 위해서입니다. 약 [10배](http://www.developerphil.com/parcelable-vs-serializable/) 빠른 성능을 냅니다.  



[MovieParcelable.java]
```java
package com.example.bagic;

import android.os.Parcel;
import android.os.Parcelable;

public class MovieParcelable implements Parcelable {

    private String name;
    private double rate;

    public MovieParcelable(String name, double rate) {
        this.name = name;
        this.rate = rate;
      }

    protected MovieParcelable(Parcel in) {
        this.name = in.readString();
        this.rate = in.readDouble();
      }

    public String getName() {
        return name;
      }

      public void setName(String name) {
        this.name = name;
      }

      public double getRate() {
        return rate;
      }

      public void setRate(double rate) {
        this.rate = rate;
      }

    @Override
    public int describeContents() {
        return 0;
    }

    @Override
    public void writeToParcel(Parcel dest, int flags) {
          dest.writeString(this.name);
          dest.writeDouble(this.rate);
    }

    public static final Parcelable.Creator<MovieParcelable> CREATOR = new Parcelable.Creator<MovieParcelable>() {

        @Override
        public MovieParcelable createFromParcel(Parcel source) {
                  return new MovieParcelable(source);
        }

        @Override
        public MovieParcelable[] newArray(int size) {
                  return new MovieParcelable[size];
        }
    };
}
```
***

>### Serializable

Serializable은 자바의 표준 인터페이스로, Android SDK에 포함되지 않습니다. 구현방법 또한 간단합니다.
Serializable은 구현이 간단하지만, 내부적으로 reflection이 발생함으로, Garbage Collection이 돌아 앱 성능을 낮추고, 베터리를 더 많이 사용하게 됩니다.

[MovieSerializable.java]
```java
package com.example.bagic;

import java.io.Serializable;

public class MovieSerializable implements Serializable {

    private String name;
    private double rate;

    public MovieSerializable(String name, double rate) {
        this.name = name;
        this.rate = rate;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getRate() {
        return rate;
    }

    public void setRate(double rate) {
        this.rate = rate;
    }
}
```
___

# Output

![Main 화면]({{ "/assets/images/parcelabe_main.png" | absolute_url }})

![Main 화면]({{ "/assets/images/parcelabe_display.png" | absolute_url }})

# Reference
https://android.jlelse.eu/parcelable-vs-serializable-6a2556d51538
http://www.developerphil.com/parcelable-vs-serializable/


# source
전체 소스 : [GitHub](https://github.com/peterkimlab/BottomAppBar)
