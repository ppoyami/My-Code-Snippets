# React Types

## state

```typescript
const [guests, setGuests] = useState<string[]>([]);
const [user, setUser] = useState<{ name: string; age: number } | undefined>();
```

## props

```typescript
interface ChildProps {
  color: string;
  onClick: () => void;
}

export const Child = ({ color, onClick }: ChildProps) => {
  return (
    <div>
      {color}
      <button onClick={onClick}>Click me</button>
    </div>
  );
};

// React.FC 는 내장 타입을 지원
export const ChildAsFC: React.FC<ChildProps> = ({
  color,
  onClick,
  children,
}) => {
  return (
    <div>
      {color}
      {children}
      <button onClick={onClick}>Click me</button>
    </div>
  );
};
```

## event

```typescript
const EventComponent: React.FC = () => {
  const onChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    console.log(event);
  };

  const onDragStart = (event: React.DragEvent<HTMLDivElement>) => {
    console.log(event);
  };

  return (
    <div>
      <input onChange={onChange} />
      <div draggable onDragStart={onDragStart}>
        Drag Me!
      </div>
    </div>
  );
};

export default EventComponent;
```

## ref

```typescript
const inputRef = useRef<HTMLInputElement | null>(null);

useEffect(() => {
    if (!inputRef.current) {
      // ! Type Guard Clause
      return;
    }

    inputRef.current.focus();
  }, []);

return (
    <div>
      User Search
      <input
        ref={inputRef}
        value={name}
        onChange={e => setName(e.target.value)} // ! inline 이기 때문에 e 는 타입추론이 된다.
      />
```

