# ゴール分解・計画ツール プログラム仕様書

## プログラム概要

### アーキテクチャ概念図

```
+----------------------------------------------+
|             ユーザーインターフェース           |
|  +--------+   +-------------+  +---------+   |
|  |ゴール設定|  |マイルストーン|  |アクション|   |
|  +--------+   +-------------+  +---------+   |
|  +------------+  +-------------+             |
|  |タイムライン |  |    設定     |             |
|  +------------+  +-------------+             |
+----------------------------------------------+
                    |
                    v
+-----------------------------------------------+
|            ビジネスロジック層                   |
|  +---------+  +-------------+  +----------+   |
|  |ゴール管理|  |マイルストーン|  |アクション |   |
|  |ロジック  |  |  ロジック   |  | ロジック  |   |
|  +---------+  +-------------+  +----------+   |
|  +-----------+  +-------------+               |
|  |タイムライン | | データ管理   |               |
|  | ロジック   |  |  ロジック    |              |
|  +-----------+  +-------------+               |
+------------------------------------------------+
                    |
                    v
+------------------------------------------+
|               データ層                    |
|                                          |
|   +-------------------------------+      |
|   |      LocalStorage (JSON)      |      |
|   +-------------------------------+      |
|                                          |
+------------------------------------------+
```

### 使用技術スタック

1. **フロントエンド**
   - HTML5: ページ構造の定義
   - CSS3: スタイリングと視覚的デザイン
   - JavaScript: ビジネスロジックとUIインタラクション

2. **データ保存**
   - ブラウザ LocalStorage API: ユーザーデータの永続化

3. **デザイン**
   - レスポンシブデザイン: モバイル、タブレット、デスクトップ対応
   - CSS変数: テーマ設定と一貫性の確保
   - Flexbox/Grid: レイアウト

4. **UI/UX機能**
   - タブベースのナビゲーション
   - モーダルウィンドウ
   - プログレスバー
   - タイムラインビジュアライゼーション

### 依存関係

- 外部ライブラリ依存なし（ピュアJavaScript実装）
- ブラウザの標準機能のみを使用
  - LocalStorage API
  - File API (エクスポート/インポート機能)
  - Modern CSS (変数、Flexbox、Grid)

## プログラム構造

### モジュール構成

プログラムは論理的に以下のモジュールに分かれています：

1. **UIコントローラ**
   - タブ管理
   - モーダル表示制御
   - フォーム入力管理

2. **データモデル**
   - ゴールデータモデル
   - マイルストーンモデル
   - アクションモデル
   - 設定モデル

3. **データ永続化**
   - LocalStorage保存/読み込み
   - エクスポート/インポート

4. **ビジネスロジック**
   - ゴール管理
   - マイルストーン管理
   - アクション管理
   - 進捗計算

5. **レンダリングエンジン**
   - ゴール表示
   - マイルストーン表示
   - アクション表示
   - タイムライン表示

### クラス階層

実際のコードではクラスは明示的に使用されていませんが、論理的には以下のオブジェクト構造となります：

```
GoalData
  ├── title
  ├── description
  ├── deadline
  ├── workGoal
  ├── lifeGoal
  ├── backcast
  │     ├── month9
  │     ├── month6
  │     └── month3
  ├── milestones[]
  │     ├── id
  │     ├── title
  │     ├── description
  │     ├── deadline
  │     └── type
  ├── actions[]
  │     ├── id
  │     ├── milestoneId
  │     ├── title
  │     ├── time
  │     ├── when
  │     ├── where
  │     ├── how
  │     ├── completed
  │     └── completedDate
  └── settings
        └── reminders
              ├── daily
              ├── weekly
              └── monthly
```

### データフロー

```
ユーザー入力 → 
  UIコントローラ → 
    データモデル更新 → 
      LocalStorage保存 → 
        レンダリング更新 → 
          ユーザー画面表示
```

1. ユーザーがフォームから入力またはアクションを実行
2. イベントハンドラがデータモデル（goalData）を更新
3. saveData()関数によりLocalStorageにデータ保存
4. renderAll()関数により画面の表示を更新
5. ユーザーに更新された情報が表示される

## 関数一覧

### 主要関数の概要表

| 関数名 | 用途 | 入力 | 出力 |
|--------|------|------|------|
| loadData() | LocalStorageからデータ読み込み | なし | なし |
| saveData() | データをLocalStorageに保存 | なし | なし |
| renderGoal() | ゴール情報を画面表示 | なし | なし |
| renderMilestones() | マイルストーン情報を画面表示 | なし | なし |
| renderActions() | アクション情報を画面表示 | なし | なし |
| renderTimeline() | タイムライン情報を画面表示 | なし | なし |
| renderAll() | 全画面表示更新 | なし | なし |
| calculateMilestoneProgress() | マイルストーンの進捗率計算 | milestoneId | 進捗率(%) |
| calculateOverallProgress() | 全体の進捗率計算 | なし | 進捗率(%) |
| formatDate() | 日付のフォーマット変換 | dateString | フォーマット済み日付文字列 |
| generateId() | ユニークID生成 | なし | ID文字列 |
| addMilestone() | マイルストーン追加 | なし | なし |
| addAction() | アクション追加 | なし | なし |
| toggleActionComplete() | アクション完了状態切替 | actionId | なし |
| showActionDetail() | アクション詳細表示 | actionId | なし |
| editMilestone() | マイルストーン編集 | milestoneId | なし |
| deleteMilestone() | マイルストーン削除 | milestoneId | なし |
| editAction() | アクション編集 | actionId | なし |
| deleteAction() | アクション削除 | actionId | なし |

### 公開API/インターフェース

このアプリケーションは単一のHTMLファイルとして実装されているため、外部向けのAPIはありませんが、以下のインターフェースを提供します：

#### LocalStorage インターフェース
- キー: `goalPlannerData`
- 値: JSON形式のゴールデータ

#### ファイルエクスポート/インポートインターフェース
- フォーマット: JSON形式
- ファイル名: `goal-planner-data.json`

## 関数詳細

### 各関数の詳細仕様

#### データ管理関数

##### loadData()
```javascript
/**
 * ローカルストレージからデータをロード
 * 保存されたデータがある場合、goalDataを更新してUI再描画
 */
function loadData() {
    const savedData = localStorage.getItem('goalPlannerData');
    if (savedData) {
        goalData = JSON.parse(savedData);
        renderAll();
    }
}
```

##### saveData()
```javascript
/**
 * データをローカルストレージに保存
 * goalDataオブジェクトをJSON文字列に変換して保存
 */
function saveData() {
    localStorage.setItem('goalPlannerData', JSON.stringify(goalData));
}
```

#### レンダリング関数

##### renderGoal()
```javascript
/**
 * ゴール情報を画面に反映
 * goalDataの内容をフォーム要素に設定
 */
function renderGoal() {
    document.getElementById('goal-title').value = goalData.title;
    document.getElementById('goal-description').value = goalData.description;
    document.getElementById('goal-deadline').value = goalData.deadline;
    document.getElementById('work-goal').value = goalData.workGoal;
    document.getElementById('life-goal').value = goalData.lifeGoal;
    document.getElementById('month-9').value = goalData.backcast.month9;
    document.getElementById('month-6').value = goalData.backcast.month6;
    document.getElementById('month-3').value = goalData.backcast.month3;
}
```

##### renderMilestones()
```javascript
/**
 * マイルストーン情報を画面に反映
 * マイルストーンリストとセレクトボックスを更新
 * マイルストーンごとの進捗状況も表示
 */
function renderMilestones() {
    const milestoneList = document.getElementById('milestone-list');
    milestoneList.innerHTML = '';

    // マイルストーンセレクトボックスの更新
    const milestoneSelect = document.getElementById('action-milestone');
    milestoneSelect.innerHTML = '<option value="">マイルストーンを選択してください</option>';

    if (goalData.milestones.length === 0) {
        milestoneList.innerHTML = '<p>まだマイルストーンがありません。マイルストーンを追加してください。</p>';
        return;
    }

    // 日付順に並べ替え
    const sortedMilestones = [...goalData.milestones].sort((a, b) => new Date(a.deadline) - new Date(b.deadline));

    // マイルストーンの表示処理
    // ...（以下省略）
}
```

##### renderActions()
```javascript
/**
 * アクション情報を画面に反映
 * マイルストーン別にグループ化して表示
 * 完了状態によってスタイル変更
 */
function renderActions() {
    const actionList = document.getElementById('action-list');
    actionList.innerHTML = '';

    if (goalData.actions.length === 0) {
        actionList.innerHTML = '<p>まだアクションがありません。アクションを追加してください。</p>';
        return;
    }

    // マイルストーン別にグループ化
    const actionsByMilestone = {};
    goalData.actions.forEach(action => {
        if (!actionsByMilestone[action.milestoneId]) {
            actionsByMilestone[action.milestoneId] = [];
        }
        actionsByMilestone[action.milestoneId].push(action);
    });

    // グループ化されたアクションの表示処理
    // ...（以下省略）
}
```

##### renderTimeline()
```javascript
/**
 * タイムライン情報を画面に反映
 * ゴール、バックキャスト、マイルストーン、完了アクションを時系列表示
 * 全体進捗率も更新
 */
function renderTimeline() {
    const timelineView = document.getElementById('timeline-view');
    timelineView.innerHTML = '';

    // 全体の進捗率を更新
    const overallProgress = calculateOverallProgress();
    document.getElementById('overall-progress').style.width = `${overallProgress}%`;
    document.querySelector('.progress-text').textContent = `${overallProgress}% 完了`;

    // ゴール、バックキャスト、マイルストーン、アクションの表示処理
    // ...（以下省略）
}
```

##### renderAll()
```javascript
/**
 * すべての要素を画面に反映
 * 個別のレンダリング関数をまとめて呼び出し
 */
function renderAll() {
    renderGoal();
    renderMilestones();
    renderActions();
    renderTimeline();
}
```

#### ユーティリティ関数

##### formatDate()
```javascript
/**
 * 日付のフォーマット
 * @param {string} dateString - YYYY-MM-DD形式の日付文字列
 * @return {string} 「YYYY年MM月DD日」形式の日付文字列
 */
function formatDate(dateString) {
    if (!dateString) return '日付未設定';
    
    const date = new Date(dateString);
    const year = date.getFullYear();
    const month = date.getMonth() + 1;
    const day = date.getDate();
    
    return `${year}年${month}月${day}日`;
}
```

##### generateId()
```javascript
/**
 * ユニークIDの生成
 * タイムスタンプとランダム文字列を組み合わせて一意のIDを生成
 * @return {string} 生成されたID
 */
function generateId() {
    return Date.now().toString(36) + Math.random().toString(36).substr(2, 5);
}
```

##### calculateMilestoneProgress()
```javascript
/**
 * マイルストーンの進捗率計算
 * @param {string} milestoneId - マイルストーンID
 * @return {number} 進捗率（0-100の整数）
 */
function calculateMilestoneProgress(milestoneId) {
    const relatedActions = goalData.actions.filter(action => action.milestoneId === milestoneId);
    if (relatedActions.length === 0) return 0;
    
    const completedCount = relatedActions.filter(action => action.completed).length;
    return Math.round((completedCount / relatedActions.length) * 100);
}
```

##### calculateOverallProgress()
```javascript
/**
 * 全体の進捗率計算
 * @return {number} 進捗率（0-100の整数）
 */
function calculateOverallProgress() {
    if (goalData.actions.length === 0) return 0;
    
    const completedCount = goalData.actions.filter(action => action.completed).length;
    return Math.round((completedCount / goalData.actions.length) * 100);
}
```

#### データ操作関数

##### addMilestone()
```javascript
/**
 * マイルストーンの追加
 * フォームから値を取得し、新しいマイルストーンを追加
 * データを保存し、UI再描画
 */
function addMilestone() {
    const title = document.getElementById('milestone-title').value.trim();
    const description = document.getElementById('milestone-description').value.trim();
    const deadline = document.getElementById('milestone-deadline').value;
    const type = document.getElementById('milestone-type').value;
    
    if (!title) {
        alert('マイルストーンのタイトルを入力してください。');
        return;
    }
    
    if (!deadline) {
        alert('達成期限を設定してください。');
        return;
    }
    
    const newMilestone = {
        id: generateId(),
        title,
        description,
        deadline,
        type
    };
    
    goalData.milestones.push(newMilestone);
    saveData();
    renderAll();
    
    // フォームをリセット
    // ...
}
```

##### addAction()
```javascript
/**
 * アクションの追加
 * フォームから値を取得し、新しいアクションを追加
 * データを保存し、UI再描画
 */
function addAction() {
    const milestoneId = document.getElementById('action-milestone').value;
    const title = document.getElementById('action-title').value.trim();
    const time = document.getElementById('action-time').value;
    const when = document.getElementById('action-when').value.trim();
    const where = document.getElementById('action-where').value.trim();
    const how = document.getElementById('action-how').value.trim();
    
    if (!milestoneId) {
        alert('関連するマイルストーンを選択してください。');
        return;
    }
    
    if (!title) {
        alert('アクションのタイトルを入力してください。');
        return;
    }
    
    const newAction = {
        id: generateId(),
        milestoneId,
        title,
        time,
        when,
        where,
        how,
        completed: false,
        completedDate: null
    };
    
    goalData.actions.push(newAction);
    saveData();
    renderAll();
    
    // フォームをリセット
    // ...
}
```

##### toggleActionComplete()
```javascript
/**
 * アクションの完了状態を切り替え
 * @param {string} actionId - アクションID
 */
function toggleActionComplete(actionId) {
    const actionIndex = goalData.actions.findIndex(action => action.id === actionId);
    if (actionIndex !== -1) {
        goalData.actions[actionIndex].completed = !goalData.actions[actionIndex].completed;
        
        // 完了日の設定/解除
        if (goalData.actions[actionIndex].completed) {
            goalData.actions[actionIndex].completedDate = new Date().toISOString().split('T')[0];
        } else {
            goalData.actions[actionIndex].completedDate = null;
        }
        
        saveData();
        renderAll();
    }
}
```

##### showActionDetail()
```javascript
/**
 * アクション詳細の表示
 * @param {string} actionId - アクションID
 * アクションの詳細情報をモーダルに表示
 */
function showActionDetail(actionId) {
    const action = goalData.actions.find(a => a.id === actionId);
    if (!action) return;
    
    // アクション詳細モーダルの内容を設定
    const milestone = goalData.milestones.find(m => m.id === action.milestoneId);
    const detailContent = document.getElementById('action-detail-content');
    
    // モーダル内容の構築と表示
    // ボタンのイベントハンドラを設定
}
```

##### editMilestone()
```javascript
/**
 * マイルストーンの編集
 * @param {string} milestoneId - マイルストーンID
 * 編集対象のマイルストーン情報をフォームに設定
 * 保存ボタンのイベントハンドラを更新用に変更
 */
function editMilestone(milestoneId) {
    const milestone = goalData.milestones.find(m => m.id === milestoneId);
    if (!milestone) return;
    
    // フォームに値を設定
    // ボタン動作の変更
}
```

##### deleteMilestone()
```javascript
/**
 * マイルストーンの削除
 * @param {string} milestoneId - マイルストーンID
 * 確認後、マイルストーンと関連するアクションを削除
 */
function deleteMilestone(milestoneId) {
    if (!confirm('このマイルストーンを削除してもよろしいですか？\n関連するアクションも削除されます。')) return;
    
    // 関連するアクションを削除
    goalData.actions = goalData.actions.filter(action => action.milestoneId !== milestoneId);
    
    // マイルストーンを削除
    goalData.milestones = goalData.milestones.filter(milestone => milestone.id !== milestoneId);
    
    saveData();
    renderAll();
}
```

##### editAction()
```javascript
/**
 * アクションの編集
 * @param {string} actionId - アクションID
 * 編集対象のアクション情報をフォームに設定
 * 保存ボタンのイベントハンドラを更新用に変更
 */
function editAction(actionId) {
    const action = goalData.actions.find(a => a.id === actionId);
    if (!action) return;
    
    // フォームに値を設定
    // ボタン動作の変更
}
```

##### deleteAction()
```javascript
/**
 * アクションの削除
 * @param {string} actionId - アクションID
 * アクションを削除しデータ保存とUI更新
 */
function deleteAction(actionId) {
    goalData.actions = goalData.actions.filter(action => action.id !== actionId);
    saveData();
    renderAll();
}
```

### パラメータ説明

#### ゴールデータモデル
- **title**: string - ゴールのタイトル
- **description**: string - ゴールの詳細説明
- **deadline**: string - 達成期限（YYYY-MM-DD形式）
- **workGoal**: string - 仕事面のゴール
- **lifeGoal**: string - 生活面のゴール
- **backcast**: object - バックキャスティング情報
  - **month9**: string - 9ヶ月後の状態
  - **month6**: string - 6ヶ月後の状態
  - **month3**: string - 3ヶ月後の状態
- **milestones**: array - マイルストーン配列
- **actions**: array - アクション配列
- **settings**: object - アプリケーション設定

#### マイルストーンモデル
- **id**: string - ユニークID
- **title**: string - マイルストーンのタイトル
- **description**: string - 詳細説明
- **deadline**: string - 達成期限（YYYY-MM-DD形式）
- **type**: string - 種類（'work', 'life', 'both'のいずれか）

#### アクションモデル
- **id**: string - ユニークID
- **milestoneId**: string - 関連マイルストーンのID
- **title**: string - アクションのタイトル
- **time**: string - 所要時間（'5min', '10min', '15min', '30min', '1h', '2h'のいずれか）
- **when**: string - 実行するタイミング
- **where**: string - 実行場所
- **how**: string - 実行方法
- **completed**: boolean - 完了状態
- **completedDate**: string|null - 完了日（YYYY-MM-DD形式、未完了の場合はnull）

### 戻り値と例外処理

- 多くの関数は戻り値を持たず、直接UIを更新する副作用を持つ
- ユーザー入力の検証は関数内で行われ、不適切な入力に対しては`alert()`を使用して警告
- データ操作前に確認ダイアログを表示（例：deleteMilestone()）
- 存在しないIDに対するアクセスは早期リターンで処理（例：`if (!action) return;`）

### 内部アルゴリズム説明

#### 進捗率計算アルゴリズム
1. 関連するアクションの総数を取得
2. 完了状態のアクションの数を取得
3. 完了数 ÷ 総数 × 100 を計算
4. 小数点以下を四捨五入して整数値として返す

#### タイムライン構築アルゴリズム
1. ゴールとバックキャスティング情報をタイムラインに追加
2. マイルストーンをタイムラインアイテムに変換
3. 完了済みアクションをタイムラインアイテムに変換
4. すべてのタイムラインアイテムを日付でソート
5. ソートされたアイテムを順番に表示

#### ユニークID生成アルゴリズム
1. 現在のタイムスタンプを取得しBase36に変換
2. ランダムな文字列を生成しBase36に変換
3. 両者を連結して一意のIDとして返す

## ライセンス
個人の範囲でご使用ください。
```
Copyright (c) 2025 Daily Growth
https://yourworklifedesign.blogspot.com/
All rights reserved.
```