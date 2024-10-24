# Flyweight 享元模式
>what(是什么): 被共享的单元<br>
>when(什么时候用): 享元对象是不可变对象。<br>
>why(为什么用)：能够复用对象，节省内存<br>
>how(如何用): 通过工厂模式，在工厂类中，通过一个 Map 来缓存已经创建过的享元对象<br>

个人理解：主要作用就是能够复用创建的对象以达到节省内存的方式

享元模式 vs 单例、缓存、对象池
- 单例模式中，一个类只能创建一个对象，而在享元模式中，一个类可以创建多个对象，每个对象被多处代码引用共享
- 缓存，主要是为了提高访问效率，而非复用
- 池化技术中的“复用”可以理解为“重复使用”，主要目的是节省时间

``` 
<!-- 享元模式 -->
<!-- 对于一篇文章来说，字体基本上没有特别的变化，对于字体可以采用享元模式，采用工厂模型+Map 来实现 -->
public class CharacterStyle {
  private Font font;
  private int size;
  private int colorRGB;

  public CharacterStyle(Font font, int size, int colorRGB) {
    this.font = font;
    this.size = size;
    this.colorRGB = colorRGB;
  }

  @Override
  public boolean equals(Object o) {
    CharacterStyle otherStyle = (CharacterStyle) o;
    return font.equals(otherStyle.font)
            && size == otherStyle.size
            && colorRGB == otherStyle.colorRGB;
  }
}

public class CharacterStyleFactory {
  private static final List<CharacterStyle> styles = new ArrayList<>();

  public static CharacterStyle getStyle(Font font, int size, int colorRGB) {
    CharacterStyle newStyle = new CharacterStyle(font, size, colorRGB);
    for (CharacterStyle style : styles) {
      if (style.equals(newStyle)) {
        return style;
      }
    }
    styles.add(newStyle);
    return newStyle;
  }
}

public class Character {
  private char c;
  private CharacterStyle style;

  public Character(char c, CharacterStyle style) {
    this.c = c;
    this.style = style;
  }
}

public class Editor {
  private List<Character> chars = new ArrayList<>();

  public void appendCharacter(char c, Font font, int size, int colorRGB) {
    Character character = new Character(c, CharacterStyleFactory.getStyle(font, size, colorRGB));
    chars.add(character);
  }
}

```



