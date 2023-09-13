---
{"dg-publish":true,"dg-show-backlinks":true,"grade":2,"permalink":"/brian-s-cabinets/","dgShowBacklinks":true,"dgPassFrontmatter":true}
---

Since 2015 I have worked with Brian's Cabinets, a large regional custom cabinet manufacturer, to build a web application for quoting, contracting, and internal processing of cabinetry jobs.

Building custom cabinets is a complex job. Proper quoting for a job needs to account for cabinet material, style, hardware, and many other details. This application takes everything into account and creates an accurate quote for a project. From there, Brian's Cabinets staff can generate proposal and contract paperwork, and prepare the job for internal manufacturing processes.

![2023-09-12_17-52.png](/img/user/98-attachments/2023-09-12_17-52.png)

## Selected Features

* Set up a new project and enter basic information
* Quote all specifications and line items for cabinetry in each room in a house (project).
* Copy and reuse cabinet specifications in a project if desired
* Application generates an accurate quote based on internal pricing data
* Print a proposal document (PDF) including optional items, and a contract with final project details
* Print PDF paperwork for internal use in scheduling, manufacturing, and billing
* Some project events trigger in-app actions or custom emails to be sent to relevant personnel
* Project templating
* Powerful reporting module

## Tech Stack

* **Front End:** Angular.js, Socket.io, SCSS
* **Deployment:** Git
* **Back End:** Node.js, Express
* **PDF:** Handlebars, node-html-pdf
* **Database:** RethinkDB
* **Server:** Ubuntu VPS

## Code Samples

### HTML / Angular.js

```html
<h4>
    Room Specifications
    <a href="#" ng-disabled="froz" class="small button" ng-click="copySpecsModal()"><i class="fa fa-copy"></i> Copy specs to other rooms</a>
</h4>
<div class="specifications">
    <!-- interfaceUpdater -->
    <div
        ng-repeat="list in lists"
        ng-if="list.order > 0 && (
          (
            proj.rooms[currentRoom.index].cabinetstyleDescription !== 'BC closet system'
          ) ||
          (
            proj.rooms[currentRoom.index].cabinetstyleDescription === 'BC closet system' &&
            [
              'doorstylebase', 'doorstiles', 'doorrails', 'uppercabinetheight',
              'uppercabinetheight2', 'factorscabinets', 'finishedends', 'finishedbacks',
              'finishedbacks2', 'crownmolding', 'basecabinetbacks', 'subtops', 'factorsdoors'
            ].indexOf(list.id) === -1
          )
        )">
      <label>
        <span>{{list.description}}</span>
        <ui-select
            ng-disabled="froz"
            ng-model="proj.rooms[currentRoom.index][list.id+'Description']"
            on-select="saveCurrentRoomField(list.id+'Description', false, 'ui-select')"
>    
          <ui-select-match placeholder="Search...">
            {{proj.rooms[currentRoom.index][list.id+'Description']}}
          </ui-select-match>
          <ui-select-choices
              repeat="item.Description as item in (list.list | filter: $select.search) track by item.id">
              {{item.Description}}
          </ui-select-choices>
        </ui-select>
      </label>
      <em ng-if="list.description == 'Finish' && proj.basic.finishSample == 'need'">
        Note: Finish sample must be approved by customer before this job can be contracted
      </em>
    </div>
</div>
```

### Angular.js

```javascript
$scope.deleteRoom = function() {
    $http
        .post('project/deleteRoom', { // Post to backend API
            idprojects: $rootScope.idprojects,
            idrooms: $scope.currentRoom.idrooms
        })
        .then(() => {
            // Remove from frontend
            $scope.proj.rooms.splice($scope.currentRoom.index, 1);

            $rootScope.log({
                summary: '[Quote] Delete Room',
                idprojects: $rootScope.idprojects,
                room: $scope.currentRoom
            });

            // Return to first room
            // This function saves the current room by default
            // (second param). I don't want to do that in this
            // case because the current room has been deleted
            $scope.setCurrentRoom($scope.proj.rooms[0], false);
            $scope.deleteRoomModalOpen = false;
            
            // Recalc project price
            $http
                .post('/pricing/calculateProjectPrice', {
                    idprojects: $rootScope.idprojects
                })
                .then(r => {
                    $scope.$parent.recalcedProjTotals = r.data;
                });
        }, (err) => { // Error
            $rootScope.errorModal = true;
            $rootScope.curError = {
                description: 'Error deleting room',
                err: err
            };
        });
};
```

### Node.js

```javascript
archiveProject(setTo, callback) {
    // Use custom RethinkDB class with utility functions
    const rethink = new Rethink(this.id, 'projects');
    rethink.fetchSingle(proj => {
        rethink.updateSingle(
            {
                id: proj.id,
                basic: {
                    archive: setTo
                }
            },
            () => {
                // Send to other frontend clients
                this.io.emit('project.archive', {
                    idprojects: this.id
                });
                callback({
                      id: this.id
                      basic: {
                          archive: setTo
                      }
                });
            }
        );
    });
}
```