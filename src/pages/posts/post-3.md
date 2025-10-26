---
layout: ../../layouts/MarkdownPostLayout.astro
title: "Reactのカスタムフックで再利用性を高める"
author: "Tech Blog"
description: "Reactのカスタムフックを使って、ロジックを再利用可能にする方法とベストプラクティスを解説します"
image:
  url: "https://docs.astro.build/assets/rays.webp"
  alt: "Reactのイメージ"
pubDate: 2024-08-10
tags: ["React", "フロントエンド", "カスタムフック"]
---

Reactのカスタムフックは、コンポーネント間でロジックを共有するための強力な仕組みです。適切に活用することで、コードの再利用性と可読性を大幅に向上させることができます。

## カスタムフックの基本

カスタムフックは、`use`で始まる名前の関数として定義します。内部でReactの組み込みフックを使用できます。

```typescript
import { useState, useEffect } from 'react';

function useFetch<T>(url: string) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [url]);

  return { data, loading, error };
}
```

## 使用例

カスタムフックを使うことで、コンポーネントはシンプルに保てます。

```typescript
function UserProfile({ userId }: { userId: string }) {
  const { data, loading, error } = useFetch<User>(`/api/users/${userId}`);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  if (!data) return null;

  return <div>{data.name}</div>;
}
```

## まとめ

カスタムフックを活用することで、ロジックを分離し、テストしやすく保守性の高いコードを書くことができます。
