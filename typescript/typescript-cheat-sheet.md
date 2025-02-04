## types vs. interface
  - mostly interchangable, there are some advantages to using interface with more robust codebases

``` typescript
type Foo = string | number | null;
type Bar = {
  userId: string;
}

interface Baz {
  userId: string;
}

// means X can be either Baz or Foo
type X = Baz | Foo;

type XX = Baz & {
  password: string;
};
// XX is the combined types of Baz and the defined object, meaning:
type XX = {
  userId: string;
  password: string;
}


interface Y extends Baz {
  password?: string;
}

// ^ this means Y is effectively
interface Y {
  userId: string;
  password?: string;
}
// because `extends` folds in the attributes of Baz as a sort of base template
```

## typing objects & optional keys

``` typescript
interface User {
  username: string | undefined; // not required, can be undefined or string (not null)
  email: string;
  firstname?: string | null; // not required, can be undefined or string or null
  address: {
    street: string;
    city: string;
    zipcode: number;
    [key: string]: number | string;
  }
}

type Users = User[] // or Array<User>
```

## generic - must pass in the type to determine / enforce types
```typescript
const LogInButton: React.FC<{ hasAccount: boolean; }> = ({ hasAccount }) => {
  return hasAccount ? (
    <LogIn />
  ) : (
    <SignUp />
  );
}
```

in practice: `LogInButton` will show error if not passed `hasAccount` or if `hasAccount` isn't a boolean
```typescript
// invalid
<LogInButton hasAccount={'foo'} />
<LogInButton hasAccount={'false'} />
<LogInButton hasAccount={null} />
<LogInButton hasAccount={undefined} />
<LogInButton foo={true} />

// valid
<LogInButton hasAccount={true} />
<LogInButton hasAccount={false} />
<LogInButton /> // technically valid because booleans in react components are false if not present
```

the function returns a promise of an array of users
```typescript
function fetchUsers(): Promise<User[]> {
  // if the type this function returns isn't an array of User's, there will be a typescript error
  return api.getUsers();
}
```

## function return types
```typescript
type Address = User['address'];

// Address is the type that is returned
function getUserAddress(user: User): Address {
  return user.address;
}

// will return either an array of strings or null
function validatePayload(payload: User): string[] | null {
  if (payloadHasErrors) {
    return ['payload is invalid'];
  }

  return null;
}

// defining variables
const validationErrors: string[] | null = validatePayload(payload);

// if validationErrors was supposed to be anything other than a string or
// null, typescript would show an error

// would show error since the return type of validatePayload is not a number
const validationErrors: number = validatePayload(payload);

// void means no value is returned
// this function doesn't return anything
const translateArg = (arg: string): void => {
  // a side effect is likely called with void returning functions
  arg = doSomething(arg);
}

```
