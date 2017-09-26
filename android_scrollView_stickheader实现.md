# android_scrollView_stickheader实现


```java

  public void setStickyHeaderIndices(int[] stickyHeaderIndices) {
    mStickyHeaderIndices = stickyHeaderIndices;
  }

  private View getSubViewByIndex(int ix) {
    ViewGroup viewGroup = (ViewGroup) getChildAt(0);
    return viewGroup.getChildAt(ix);
  }


  private int getSubViewCount() {
    if (getChildCount() == 0) {
      return 0;
    }
    return ((ViewGroup) getChildAt(0)).getChildCount();
  }

  private int[] mStickyHeaderIndices;
  private boolean allowStickHeader() {
    return mStickyHeaderIndices != null && mStickyHeaderIndices.length != 0;
  }

  /**
    * 主要实现方法
    */
  private void scrollStickHeader(int l, int t, int oldl, int oldt) {
    if (!allowStickHeader()) {
      return;
    }

    int subViewCount = getSubViewCount();

    View previousView = null;
    View currentView = null;
    View nextView = null;

    for (int i = 0; i < mStickyHeaderIndices.length; i++) {
      int idx = mStickyHeaderIndices[i];
      if (idx >= subViewCount) {
        continue;
      }

      View header = getSubViewByIndex(idx);


      if (nextView == null) {
        int top = header.getTop();
        if (top > t) {
          nextView = header;
        } else {
          previousView = currentView;
          currentView = header;
        }
      }
      header.setTranslationZ(0);
      header.setTranslationY(0);
    }

    if (currentView == null) {
      return;
    }
    int currentFrameTop = currentView.getTop();
    int currentFrameHeight = currentView.getMeasuredHeight();
    int yOffset = t - currentFrameTop;
    if (nextView != null) {
      int nextViewTop = nextView.getTop();
      int overlap = currentFrameHeight - (nextViewTop - t);
      yOffset -= Math.max(0, overlap);
    }

    currentView.setTranslationY(yOffset);
    currentView.setTranslationZ(1);
    if (previousView != null) {
      previousView.setTranslationY(0);
    }
  }


```