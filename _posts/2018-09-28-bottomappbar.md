---
title: "BottomAppBar java 코드로 구현하기"
date: 2018-09-28 14:26:28 -0400
categories: jekyll update
---

MainActivity.java는 `Activity` 로 생성하고, 화면 전환하는 View는 `Fragment`로 생성하였다.
일반적으로 appbar는 `AndroidManifest`에서 선언하는데, 이렇게 되면 BottomAppBar의 fab 애니메이션 적용이 안되서 인지, 선언이 되지 않았다. 결론은 BottomAppBar는 화면 전환 시, BottomAppBar를 detach 하고, fab를 이동 시킨 후, 다시 BottomAppBar를 붙혀야 하는 과정을 거친다.

[MainActivity.java]
```
private FragmentTransaction transaction = null;

@Override
  protected void onCreate(Bundle savedInstanceState) {
    setContentView(R.layout.activity_main);

    fabs = (FloatingActionButton) findViewById(R.id.fabs);

    transaction = getSupportFragmentManager().beginTransaction();
    transaction.replace(R.id.flContainerHome, HomeFragment.newInstance());
    transaction.commit();
  }
```
___

1. First ordered list item
2. Another item
⋅⋅* Unordered sub-list.
1. Actual numbers don't matter, just that it's a number
⋅⋅1. Ordered sub-list
4. And another item.

* Unordered list can use asterisks
- Or minuses
+ Or pluses

___
