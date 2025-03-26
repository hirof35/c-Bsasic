# include <Siv3D.hpp>  // Siv3D のヘッダー

constexpr int gridSize = 4;  // 4x4 のパズル
constexpr int tileSize = 100;  // タイルのサイズ
constexpr int emptyTile = 0;  // 空白のタイルを 0 で表す

Array<Array<int>> grid;  // パズルのグリッド

// パズルを初期化する関数
void initializePuzzle() {
    grid.clear();
    Array<int> numbers;

    // 1～15 の数字をリストに追加
    for (int i = 1; i < gridSize * gridSize; ++i) {
        numbers.push_back(i);
    }
    numbers.push_back(emptyTile);  // 最後の1つは空白

    // シャッフルして 4x4 のグリッドを作る
    do {
        std::shuffle(numbers.begin(), numbers.end(), GetRandom());
    } while (!isSolvable(numbers));  // 解ける配置になるまで繰り返す

    // 2次元配列に変換
    for (int y = 0; y < gridSize; ++y) {
        Array<int> row;
        for (int x = 0; x < gridSize; ++x) {
            row.push_back(numbers[y * gridSize + x]);
        }
        grid.push_back(row);
    }
}

// パズルが解ける状態か判定する関数
bool isSolvable(const Array<int>& numbers) {
    int inversions = 0;
    for (int i = 0; i < numbers.size(); ++i) {
        if (numbers[i] == emptyTile) continue;
        for (int j = i + 1; j < numbers.size(); ++j) {
            if (numbers[j] != emptyTile && numbers[i] > numbers[j]) {
                ++inversions;
            }
        }
    }
    return (inversions % 2 == 0);
}

// 空白の位置を取得
Point getEmptyTilePos() {
    for (int y = 0; y < gridSize; ++y) {
        for (int x = 0; x < gridSize; ++x) {
            if (grid[y][x] == emptyTile) {
                return { x, y };
            }
        }
    }
    return { -1, -1 };  // 空白がない場合
}

// タイルを動かす関数
void moveTile(Point tilePos) {
    Point emptyPos = getEmptyTilePos();

    // 隣接している場合のみ移動
    if (Abs(tilePos.x - emptyPos.x) + Abs(tilePos.y - emptyPos.y) == 1) {
        std::swap(grid[tilePos.y][tilePos.x], grid[emptyPos.y][emptyPos.x]);
    }
}

// パズルが完成しているかチェック
bool isSolved() {
    int count = 1;
    for (int y = 0; y < gridSize; ++y) {
        for (int x = 0; x < gridSize; ++x) {
            if (y == gridSize - 1 && x == gridSize - 1) return true;
            if (grid[y][x] != count++) return false;
        }
    }
    return true;
}

// メインループ
void Main() {
    initializePuzzle();  // 初期化

    while (System::Update()) {
        // タイルの描画
        for (int y = 0; y < gridSize; ++y) {
            for (int x = 0; x < gridSize; ++x) {
                Rect rect(x * tileSize, y * tileSize, tileSize - 2);
                if (grid[y][x] != emptyTile) {
                    rect.draw(ColorF(0.5, 0.7, 1.0)).drawFrame(2, 0, Palette::Black);
                    FontAsset(U"Number")(grid[y][x]).drawAt(rect.center(), Palette::White);
                }
            }
        }

        // クリック処理
        if (MouseL.down()) {
            Point mousePos = Cursor::Pos() / tileSize;
            if (mousePos.x >= 0 && mousePos.x < gridSize && mousePos.y >= 0 && mousePos.y < gridSize) {
                moveTile(mousePos);
            }
        }

        // クリア判定
        if (isSolved()) {
            FontAsset(U"Result")(U"Puzzle Solved!").drawAt(Scene::Center(), Palette::Red);
        }
    }
} 
