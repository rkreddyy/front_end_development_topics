# Angular Code Snippets

#### The example below binds the `time` Observable to the view using `async pipe`. The Observable continuously updates the view with the current time.

```
@Component({
  selector: 'async-observable-pipe',
  template: '<div>Time: {{ time | async }}</div>'
})
export class AsyncObservablePipeComponent {
  time = new Observable<string>((observer: Observer<string>) => {
    setInterval(() => observer.next(new Date().toString()), 1000);
  });
}
```

