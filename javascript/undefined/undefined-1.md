# 유틸리티 타입

## ReadOnly&lt;T&gt;

```typescript
  /**
 * Make all properties in T readonly
 */
  type Readonly<T> = {
    readonly [P in keyof T]: T[P];
  };
  
  type ToDo = {
    title: string;
    description: string;
  };

  function display(todo: Readonly<ToDo>) {
    todo.title = 'jaja'; error
  }
```

## Partial&lt;T&gt;

```typescript
  /**
 * Make all properties in T optional
 */
 
  type Partial<T> = {
    [P in keyof T]?: T[P];
  };
  
  type ToDo = {
    title: string;
    description: string;
    label: string;
    priority: 'high' | 'low';
  };

  // M: 부분적인 것들도 허용하고 싶을 떄.
  function updateTodo(todo: ToDo, fieldsToUpdate: Partial<ToDo>): ToDo {
    return { ...todo, ...fieldsToUpdate };
  }
  const todo: ToDo = {
    title: 'learn TypeScript',
    description: 'study hard',
    label: 'study',
    priority: 'high',
  };
  const updated = updateTodo(todo, { priority: 'low' });

  console.log(updated); // {...todo, priority: 'low'}
```

## Pick&lt;T, K extends keyof T&gt;, Omit&lt;T, K extends keyof T&gt;

```typescript
  type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
  };
  
  type Video = {
    id: string;
    title: string;
    url: string;
    data: string;
  };
  
  // M: 기존 타입에서 원하는 타입만 뽑아 새롭게 정의할 때 사용함
  type VideoMetadata = Pick<Video, 'id' | 'title'>;
  // M: 뺴고자 하는 것이 명확할 때 사용, 선택하고자 하는 것이 명확하면 peek
  type VideoMetadata = Omit<Video, 'url' | 'data'>;

  function getVideo(id: string): Video {
    return {
      id,
      title: 'video',
      url: 'https://..',
      data: 'byte-data..',
    };
  }
  function getVideoMetadata(id: string): VideoMetadata {
    return {
      id: id,
      title: 'title',
    };
```

## Record&lt;K extends keyof any, T&gt;

```typescript
type Record<K extends keyof any, T> = {
    [P in K]: T;
};

type PageInfo = {
  title: string;
};

type Page = 'home' | 'about' | 'contact';

// M: key: value로 연결하고 싶을 때 사용
const nav: Record<Page, PageInfo> = {
  home: { title: 'Home' },
  about: { title: 'About' },
  contact: { title: 'Contact' },
};
```

