## Tip

### Android

#### Activity

- Activity进入退出动画overridePendingTransition(int enter, int exit)只能写在startActivityxxx()和finish()之后，或者重写OnBackPressed()中

  ```java
  @Override
  public void onBackPressed() {
      super.onBackPressed();
      overridePendingTransition(R.anim.in,R.anim.out);
  }
  ```





#### Fragment

- Fragment进入退出动画使用FragmentTransaction的setCustomAnimations(int enter, int exit)设置，设置需要在装载Fragment之前，否则没有效果

  ```java
  getSupportFragmentManager().beginTransaction().setCustomAnimations(R.anim.in,R.anim.out).replace(R.id.container,new MainFragment()).commit();
  //需要放在replace或者add之前 动画效果才能生效
  ```




