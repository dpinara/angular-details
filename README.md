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
