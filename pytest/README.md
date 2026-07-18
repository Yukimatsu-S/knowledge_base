# 概要
pytest とは Python のテスト実行ツール
以下のような命名規則に従っているテストファイルを自動で探して実行する

```txt
test_*.py
*_test.py
```

## 実際の使用例
テスト関数の命名規則は `test_*` です
つまり、`test_*.py` または `*_test.py` ファイルの `test_*` と名前のつく関数を自動でテストします

```python
def test_calculation_weekly_average():
    resrult = calculate_weekly_average([1000, 2000, 3000])

    assert result == 2000
```

実際にスクリプトで使用される関数をテストを実行するための関数でラップするイメージです


## テストの基本構造
基本的にはテストは次の3段階で考える

```text
Arrange : 入力を準備する
Act : 対象関数を実行する
Assert : 結果を確認する
```

上記の例に当てはめると

```python
def test_calculation_weekly_average():
    # Arrange
    calories = [1000, 2000, 3000]

    # Act
    result = calculate_weekly_average(calories)

    # Assert
    assert result === 2000
```

# よく使う pytest の機能

## 1. assert
期待する効果と一致するか確認する

```python
def test_calculation_weekly_average():
    resrult = calculate_weekly_average([1000, 2000, 3000])

    assert result == 2000
```

## 2. pytest.approx
小数計算には丸め誤差があるため、完全一致ではなく近似比較を使う

```python
import pytest

def test_weight_change_percent():
    result = calculate_weight_change_percent(
        current_average = 69.3,
        previous_average = 70.0,
    )

    assert result == pytest.approx(-1.0)
```

`pytest.approx()` は浮動小数点の小さな誤差を許容して比較する

## 3. pytest.raises
想定した例外が発生することを確認する

```python
import pytest

def test_weight_change_rejects_zero_previous_weight():
    with pytest.raises(ValueError):
        calculate_weight_change_percent(
            current_average = 70.0,
            previous_average = 0,
        )
```
例外が発生しなければ、このテストは失敗する

## 4. pytest.mark.parametrize
同じテストを異なる入力で繰り返す

```python
import pytest

@pytest.mark.parametrize(
    "goal,weight_change,expected_direction",
    [
        ("減量", -0.7, "maintain"),
        ("減量", 0.0, "decrease"),
        ("減量", -1.5, "increase"),
        ("維持", 0.0, "maintain"),
        ("増量", 0.3, "maintain"),
    ],
)
def test_calorie_adjustment_direction(
    goal,
    weight_change,
    expected_direction,
):
    result = calculate_calorie_adjustment(
        goal=goal,
        weekly_calorie_average=2000,
        weight_change_percent=weight_change,
    )

    assert result.direction == expected_direction
```

一つのテスト関数で複数の条件を検証でき、失敗した条件も個別に表示される

## fixture
update coming soon


# pytest の導入方法
使用したい環境に以下のコマンドでインストール
```shell
pip install pytest
```

以下のコマンドで `pytest` を実行する
```shell
python -m pytest
```

以下のオプションで詳細表示
```shell
python -m pytest -v
```

特定ファイルだけ実行
```shell
python -m pytest tests/test_nutrition.py
```

特定の名前を含むテストだけ実行
```shell
python -m pytest -k weight_chnage
```
pytest は命名規則に従ったテストを自動収集する