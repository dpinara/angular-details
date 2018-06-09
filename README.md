# angular-details
angular-unittesting

    {

    import { FetchDataComponent } from './fetchdata.component';
    import { Http } from '@angular/http';
    import { TestBed, ComponentFixture } from '@angular/core/testing';

    export class MockHttp {
        get(url:string) {
            return { subscribe:()=> {} };
        }
    }

    let mockHttp:MockHttp;
    let fixture:ComponentFixture<FetchDataComponent>;

    describe('fetchdata', () => {

        beforeEach(() => {
            mockHttp = new MockHttp();
            spyOn(mockHttp, 'get').and.returnValue({subscribe:() => {}});

            TestBed.configureTestingModule({
                declarations: [FetchDataComponent],
                providers: [
                    { provide:Http, useValue:mockHttp },
                    { provide: 'BASE_URL', useValue:'' }
                ]
            });

            fixture = TestBed.createComponent(FetchDataComponent);
            fixture.detectChanges();
        });

        it('should call http.get', () => {
            expect(mockHttp.get).toHaveBeenCalled();
        });
    });

    }

# Template for component : 

    import { VoteComponent } from './vote.component'; 

    describe('VoteComponent', () => {
      var component: VoteComponent; 

      beforeEach(() => {
        component = new VoteComponent();
      });

      it('', () => {
      });
    });

# Template for pipe

        import { TextSummaryPipe } from './text-summary.pipe';

        describe('TextSummaryPipe', () => {
        let pipe: TextSummaryPipe;

        beforeEach(() => {
        pipe = new TextSummaryPipe();
        });

        it('should return an empty string if input is null', () => {
        expect (pipe.transform(null)).toEqual('');
        });

        it('should return null if input is undefined', () => {
        expect (pipe.transform(undefined)).toEqual('');
        });

        it('should return empty string if input is an empty string', () => {
        expect (pipe.transform('')).toEqual('');
        });

        it('should return the same string if the length of input is less than the limit', () => {
        expect (pipe.transform('12345', 5)).toEqual('12345');
        });

        it('should summarize the input if it is longer than the limit', () => {
        expect(pipe.transform('12345678901', 5)).toEqual('12345...');
        });

        it('should assume 10 as the limit if not given', () => {
        expect(pipe.transform('12345678901')).toEqual('1234567890...');
        });
        });

    # Template for simple non angular class/component

    import { LikeComponent } from './like.component';

    describe('LikeComponent', () => {
        let component: LikeComponent;

        beforeEach(() => {
          component = new LikeComponent();
        });

        it('should toggle the iLike property when I click it', () => {
          component.iLike = true; 

          component.click();

          expect(component.iLike).toBe(false);
        });

        it('should decrement total likes if I like an object and click the LikeComponent to unlike it', () => {
          component.iLike = true; 
          component.totalLikes = 1; 

          component.click();

          expect(component.totalLikes).toBe(0);
        });

        it('should increment total likes if I do NOT like an object and click the LikeComponent to like it', () => {
          component.iLike = false; 
          component.totalLikes = 0; 

          component.click();

          expect(component.totalLikes).toBe(1);
        });
    });

# Template for angular component unit testing, but still not doing integration testing ( not using TestBed )

    import { UsersComponent } from './users.component'; 
    import { UserService } from './user.service'; 
    import { Observable } from 'rxjs/Observable';
    import 'rxjs/add/observable/throw'; 

    describe('UsersComponent', () => {
      let component: UsersComponent; 
      let service: UserService; 

      beforeEach(() => {
        service = new UserService(null);
        component = new UsersComponent(service);
      });

      it('should set users property with the users retrieved from the server', () => {
        let users = [ 1, 2, 3 ];
        spyOn(service, 'getUsers').and.returnValue(Observable.from([ users ]));

        component.ngOnInit();

        expect(component.users).toBe(users);
      });

      describe('When deleting a user', () => {
        let user; 

        beforeEach(() => {
          component.users = [
            { id: 1 },
            { id: 2 },
          ];

          user = component.users[0]; 
        });

        it('should remove the selected user from the list if the user confirms deletion', () => {
          spyOn(window, 'confirm').and.returnValue(true);
          spyOn(service, 'deleteUser').and.returnValue(Observable.empty());

          component.deleteUser(user);

          expect(component.users.indexOf(user)).toBe(-1);
        });

        it('should NOT remove the seleted user from the list if the user cancels', () => {
          spyOn(window, 'confirm').and.returnValue(false);

          component.deleteUser(user);

          expect(component.users.indexOf(user)).toBeGreaterThan(-1);
        });

        it('should call the server to delete the selected user if the user confirms deletion', () => {
          spyOn(window, 'confirm').and.returnValue(true);
          let spy = spyOn(service, 'deleteUser').and.returnValue(Observable.empty());

          component.deleteUser(user);

          expect(spy).toHaveBeenCalledWith(user.id);
        });

        it('should NOT call the server to delete the selected user if the user cancels', () => {
          spyOn(window, 'confirm').and.returnValue(false);
          let spy = spyOn(service, 'deleteUser').and.returnValue(Observable.empty());

          component.deleteUser(user);

          expect(spy).not.toHaveBeenCalled();
        });

        it('should undo deletion if the call to the server fails', () => {
          spyOn(window, 'confirm').and.returnValue(true);
          // We need to change the implementation of alert, otherwise 
          // it will popup a dialog when running our unit tests.
          spyOn(window, 'alert').and.callFake(() => {}); 
          spyOn(service, 'deleteUser').and.returnValue(Observable.throw('error'));

          component.deleteUser(user);

          expect(component.users.indexOf(user)).toBeGreaterThan(-1);
        });

        it('should display an error if the call to the server fails', () => {
          spyOn(window, 'confirm').and.returnValue(true);
          let spy = spyOn(window, 'alert').and.callFake(() => {}); 
          spyOn(service, 'deleteUser').and.returnValue(Observable.throw('error'));

          component.deleteUser(user);

          expect(spy).toHaveBeenCalled();
        });
      });
    });


# Template to test AppModule

    /* tslint:disable:no-unused-variable */
    import { async, ComponentFixture, TestBed } from '@angular/core/testing';
    import { RouterTestingModule } from '@angular/router/testing'; 
    import { By } from '@angular/platform-browser';
    import { DebugElement } from '@angular/core';

    import { AppComponent } from './app.component';

    describe('AppComponent', () => {
      let component: AppComponent;
      let fixture: ComponentFixture<AppComponent>;

      beforeEach(async(() => {
        TestBed.configureTestingModule({
          declarations: [ AppComponent ]
        })
        .compileComponents();
      }));

      beforeEach(() => {
        fixture = TestBed.createComponent(AppComponent);
        component = fixture.componentInstance;
        fixture.detectChanges(); 
      });
    });


# Template to test component in angular way ( still need to pass service which is not done )

    /* tslint:disable:no-unused-variable */
    import { async, ComponentFixture, TestBed } from '@angular/core/testing';
    import { By } from '@angular/platform-browser';
    import { DebugElement } from '@angular/core';

    import { UserDetailsComponent } from './user-details.component';

    xdescribe('UserDetailsComponent', () => {
      let component: UserDetailsComponent;
      let fixture: ComponentFixture<UserDetailsComponent>;

      beforeEach(async(() => {
        TestBed.configureTestingModule({
          declarations: [ UserDetailsComponent ]
        })
        .compileComponents();
      }));

      beforeEach(() => {
        fixture = TestBed.createComponent(UserDetailsComponent);
        component = fixture.componentInstance;
        fixture.detectChanges();
      });

      it('should create', () => {
        expect(component).toBeTruthy();
      });
    });


# Template to test directive

      import { async, ComponentFixture, TestBed } from '@angular/core/testing';
      import { HighlightDirective } from './highlight.directive';
      import { By } from '@angular/platform-browser';
      import { Component } from '@angular/core'; 

      @Component({
        template: `
          <p highlight="cyan">First</p>
          <p highlight>Second</p>
        `
      })
      class DirectiveHostComponent { 
      }

      describe('HighlightDirective', () => {
        let fixture: ComponentFixture<DirectiveHostComponent>;

        beforeEach(async(() => {
          TestBed.configureTestingModule({
            declarations: [ DirectiveHostComponent, HighlightDirective ]
          })    
          .compileComponents();
        }));

        beforeEach(() => {
          fixture = TestBed.createComponent(DirectiveHostComponent);
          fixture.detectChanges(); 
        });
      });

# Template for service : This seems to auto generated..

    import { TestBed, inject } from '@angular/core/testing';

    import { DeactivateGuardService } from './deactivate-guard.service';

    describe('DeactivateGuardService', () => {
      beforeEach(() => {
        TestBed.configureTestingModule({
          providers: [DeactivateGuardService]
        });
      });

      it('should ...', inject([DeactivateGuardService], (service: DeactivateGuardService) => {
        expect(service).toBeTruthy();
      }));
    });
