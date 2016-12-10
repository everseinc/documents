# 目次

- [Ruby バージョン](#ruby-version)

- [インデント](#indentatio)

- [Lambda式](#lambda)

- [関数型 Ruby](#functional-ruby)

# Ruby コーディング規約

## 概要

本文は、株式会社Everseが保有するプログラムのコーディングスタイルを規定するものである。

<a name="ruby-version"></a>

## Ruby バージョン
- **【推奨】** Ruby 2.3


<a name="indentension"></a>

## インデント
- **【必須】** １インデントに対して２つの空白を使用する


<a name="lambda"></a>

## Lambda式
- **【必須】** Lambdaオブジェクトの記述は`lambda{ |x| ~ }`ではなく、シンタックスシュガーである`-> (x) { ~ }`と記述すること.

```ruby

#bad
y = lambda{ |x| x * x }

#good
y = -> (x) { x * x }

#bad
y = ->(x) { x * x }

#bad
y = -> (x){ x * x }

#bad
y = ->(x){ x * x }

#bad
y = -> (x) {x * x}

```

<a name="functional-ruby"></a>

## 関数型 Ruby
関数型Rubyに関しては、`functional_ruby`ライブラリとして提供する。

- Functor

定義 : Functor moduleをincludeしたクラス
mapをoverrideして、map処理が定義されたものをFunctorとし、Procクラスを拡張した <= メソッドでの記述を推奨

```ruby
# functor
arr = [1, 2, 3]
f = -> (x) { x * 2 }

f <= arr
#=> [2, 4, 6]
```

- Applicative

定義 : Applicative moduleをincludeしたクラス
applicateをoverrideして、applicate処理が定義されたものをApplicativeとし、alias_methodである > メソッドでの記述を推奨

```ruby
:gsub.to_proc.curry(3) <= ["hi", "hello"] > [/h/] > ["f"]

#
# 解説
#

f = :gsub.to_proc
#=> {|r, a, b| r.gsub(a, b)}

g = f.curry(3)
#=> { |r|
#=>   { |a|
#=>     { |b|
#=>       r.gsub(a, b)
#=>     }
#=>   }
#=> }

h = :gsub.to_proc.curry(3) <= ["hi", "hello"]
#=> [
#=>   { |a|
#=>     { |b|
#=>       "hi".gsub(a, b)
#=>     }
#=>   },
#=>   { |a|
#=>     { |b|
#=>       "hello".gsub(a, b)
#=>     }
#=>   }
#=> [

h = :gsub.to_proc.curry(3) <= ["hi", "hello"] > [/h/]
#=> [
#=>   { |b|
#=>     "hi".gsub(/h/, b)
#=>   },
#=>   { |b|
#=>     "hello".gsub(/h/, b)
#=>   }
#=> [

:gsub.to_proc.curry(3) <= ["hi", "hello"] > [/h/] > ["f"]
#=> [
#=>   "hi".gsub(/h/, "f"),
#=>   "hello".gsub(/h/, "f")
#=> [

#=> ["fi", "fello"]
```